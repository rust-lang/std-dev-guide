
# Building and Debugging the library crates

Most of the [instructions from the rustc-dev-guide][rustc-guide] also apply to the standard library since
it is built with the same build system, so it is recommended to read it first.

[rustc-guide]: https://rustc-dev-guide.rust-lang.org/building/how-to-build-and-run.html

# Println-debugging alloc and core

Since logging and IO APIs are not available in `alloc` and `core` advice meant for the rest of the compiler
is not applicable here.

Instead one can either extract the code that should be tested to a normal crate and add debugging statements there or
on POSIX systems one can use the following hack:

```rust
extern "C" {
    fn dprintf(fd: i32, s: *const u8, ...);
}

macro_rules! dbg_printf {
    ($s:expr) => {
        unsafe { dprintf(2, "%s\0".as_ptr(), $s as *const u8); }
    }
}

fn function_to_debug() {
    let dbg_str = format!("debug: {}\n\0", "hello world");
    dbg_printf!(dbg_str.as_bytes().as_ptr());
}
```

Then one can run a test which exercises the code to debug and show the error output via

```shell,ignore
./x.py test library/alloc --test-args <test_name> --test-args --nocapture
```