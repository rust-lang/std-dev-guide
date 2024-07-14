# Breaking changes to the prelude

Making changes to the prelude can easily cause breakage because it impacts all Rust code.
In most cases the impact is limited since prelude items have the lowest priority in name lookup (lower than glob imports), but there are three cases where this requires additional care or may not be possible.

## Traits

Adding a new trait to the prelude causes new methods to become available for existing types.
This can cause name resolution errors in user code if a method with the same name is also available from a different trait.

For this reason, [`TryFrom` and `TryInto`](https://github.com/rust-lang/rust/issues/33417) were only added to the prelude for the 2021 edition despite being stabilized in 2019.

## Macros

Unlike other item types, rustc's name resolution for macros does not support giving prelude macros a lower priority than other macros, even if the macro is unstable.
As a general rule, avoid adding macros to the prelude except at edition boundaries.

This issues was encoutered when trying to land the [`assert_matches!` macro](https://github.com/rust-lang/rust/issues/82913).

## Types

Adding a new type usually doesn't create any breakage, because prelude types have a lower priority than names in user code.

However, code that declares an enum, *and* has a derive on that enum, *and* attempts to locally import a variant from that enum (e.g. `use EnumType::Variant;`) will break if the prelude starts exporting a type with the same name as that enum.

This makes adding a new type to the prelude require a crater run to check for breakage in existing Rust code.

If Rust code declares a local variable binding and the prelude adds a type of the same name, this can likewise introduce breakage. This case is much less likely to arise, as it requires declaring a variable name using the `CamelCase` convention typically used for types, and ignoring rustc's warnings about doing so.
