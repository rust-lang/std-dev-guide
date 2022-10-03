# Library optimizations and benchmarking

Recommended reading: [The Rust performance book](https://nnethercote.github.io/perf-book/title-page.html)

## What to optimize

It's preferred to optimize code that shows up as significant in real-world code.
E.g. it's more beneficial to speed up `[T]::sort` than it is to shave off a small allocation in `Command::spawn`
because the latter is dominated by its syscall cost.

Issues about slow library code are labeled as [I-slow T-libs](https://github.com/rust-lang/rust/issues?q=is%3Aissue+is%3Aopen+label%3AI-slow+label%3AT-libs)
and those about code size as [I-heavy T-libs](https://github.com/rust-lang/rust/issues?q=is%3Aissue+is%3Aopen+label%3AI-heavy+label%3AT-libs)

## Vectorization

Currently explicit SIMD features can't be used in alloc or core because runtime feature-detection is only available in std
and they are compiled with each target's baseline feature set.

Vectorization can only be achieved by shaping code in a way that the compiler backend's auto-vectorization passes can understand.

## rustc-perf

For parts of the standard library that are heavily used by rustc itself it can be convenient to use
[the benchmark server](https://github.com/rust-lang/rustc-perf/tree/master/collector#benchmarking).

Since it only measures compile-time but not runtime performance of crates it can't be used to benchmark for features
that aren't used by the compiler, e.g. floating point code, linked lists, mpsc channels, etc.
For those explicit benchmarks must be written or extracted from real-world code.

## Built-in Microbenchmarks

The built-in benchmarks use [cargo bench](https://doc.rust-lang.org/nightly/unstable-book/library-features/test.html)
and can be found in the `benches` directory for `core` and `alloc` and in `test` modules in `std`.

The benchmarks are automatically executed run in a loop by `Bencher::iter` to average the runtime over many loop-iterations.
For CPU-bound microbenchmarks the runtime of a single iteration should be in the range of nano- to microseconds.

To run a specific  can be invoked without recompiling rustc
via `./x bench library/<lib> --stage 0 --test-args <benchmark name>`.

`cargo bench` measures wall-time. This often is good enough, but small changes such as saving a few instructions
in a bigger function can get drowned out by system noise. In such cases the following changes can make runs more
reproducible:

* disable incremental builds in `config.toml`
* build std and the benchmarks with `RUSTFLAGS_BOOTSTRAP="-Ccodegen-units=1"`
* ensure the system is as idle as possible
* [disable ASLR](https://man7.org/linux/man-pages/man8/setarch.8.html)
* [pinning](https://man7.org/linux/man-pages/man1/taskset.1.html) the benchmark process to a specific core
* [disable clock boosts](https://wiki.archlinux.org/title/CPU_frequency_scaling#Configuring_frequency_boosting),
  especially on thermal-limited systems such as laptops

## Standalone tests

If `x` or the cargo benchmark harness get in the way it can be useful to extract the benchmark into a separate crate,
e.g. to run it under `perf stat` or cachegrind.

Build and link the [stage1](https://rustc-dev-guide.rust-lang.org/building/how-to-build-and-run.html#creating-a-rustup-toolchain)
compiler as rustup toolchain and then use that to build the standalone benchmark with a modified standard library.

[Currently](https://github.com/rust-lang/rust/issues/101691) there is no convenient way to invoke a stage0 toolchain with
a modified standard library. To avoid the compiler rebuild it can be useful to not only extract the benchmark but also
the code under test into a separate crate.

## Running under perf-record

If extracting the code into a separate crate is impractical one can first build the benchmark and then run it again
under `perf record` and then drill down to the benchmark kernel with `perf report`.

```terminal,ignore
# 1CGU to reduce inlining changes and code reorderings, debuginfo for source annotations
$ export RUSTFLAGS_BOOTSTRAP="-Ccodegen-units=1 -Cdebuginfo=2"

# build benchmark without running it
$ ./x bench --stage 0 library/core/ --test-args skipallbenches

# run the benchmark under perf
$ perf record --call-graph dwarf -e instructions ./x bench --stage 0 library/core/ --test-args <benchmark name>
$ perf report
```

By renaming `perf.data` to keep it from getting overwritten by subsequent runs it can be later compared to runs with
a modified library with `perf diff`.

## comparing assembly

While `perf report` shows assembly of the benchmark code it can sometimes be difficult to get a good overview of what
changed, especially when multiple benchmarks were affected. As an alternative one can extract and diff the assembly
directly from the benchmark suite.

```terminal,ignore
# 1CGU to reduce inlining changes and code reorderings, debuginfo for source annotations
$ export RUSTFLAGS_BOOTSTRAP="-Ccodegen-units=1 -Cdebuginfo=2"

# build benchmark libs
$ ./x bench --stage 0 library/core/ --test-args skipallbenches

# this should print something like the following
Running benches/lib.rs (build/x86_64-unknown-linux-gnu/stage0-std/x86_64-unknown-linux-gnu/release/deps/corebenches-2199e9a22e7b1f4a)

# get the assembly for all the benchmarks
$ objdump --source --disassemble --wide --no-show-raw-insn --no-addresses \
  build/x86_64-unknown-linux-gnu/stage0-std/x86_64-unknown-linux-gnu/release/deps/corebenches-2199e9a22e7b1f4a \
  | rustfilt > baseline.asm

# switch to the branch with the changes
$ git switch feature-branch

# repeat the procedure above
$ ./x bench ...
$ objdump ... > changes.asm

# compare output
$ kdiff3 baseline.asm changes.asm
```

This can also be applied to standalone benchmarks.
