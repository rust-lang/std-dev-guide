# Breaking changes to the prelude

Making changes to the prelude can easily cause breakage because it impacts all Rust code. In most cases the impact is limited since prelude items have the lowest priority in name lookup (lower than glob imports), but there are two cases where this doesn't work.

## Traits

Adding a new trait to the prelude causes new methods to become available for existing types. This can cause name resolution errors in user code if a method with the same name is also available from a different trait.

For this reason, [`TryFrom` and `TryInto`](https://github.com/rust-lang/rust/issues/33417) were only added to the prelude for the 2021 edition despite being stabilized in 2019.

## Macros

Unlike other item types, rustc's name resolution for macros does not support giving prelude macros a lower priority than other macros, even if the macro is unstable. As a general rule, avoid adding macros to the prelude except at edition boundaries.

This issues was encoutered when trying to land the [`assert_matches!` macro](https://github.com/rust-lang/rust/issues/82913).
