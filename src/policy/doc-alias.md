doc alias policy
================

Rust's documentation supports adding aliases to any declaration (such as a
function, type, or constant), using the syntax `#[doc(alias = "name")]`. We
want to use doc aliases to help people find what they're looking for, while
keeping those aliases maintainable and high-value. This policy outlines the
cases where we add doc aliases, and the cases where we omit those aliases.

- We must have a reasonable expectation that people might search for the term
  in the documentation search. Rust's documentation provides a name search, not
  a full-text search; as such, we expect that people may search for plausible
  names, but that for more general documentation searches they'll turn to a web
  search engine.
  - Related: we don't expect that people are currently searching Rust
    documentation for language-specific names from arbitrary languages they're
    familiar with, and we don't want to add that as a new documentation search
    feature; please don't add aliases based on your favorite language. Those
    mappings should live in separate guides or references. We do expect that
    people might look for the Rust name of a function they reasonably expect to
    exist in Rust (e.g. a system function or a C library function), to try to
    figure out what Rust called that function.
- The proposed alias must be a name we would plausibly have used for the
  declaration. For instance, `mkdir` for `create_dir`, or `rmdir` for
  `remove_dir`, or `popcnt` and `popcount` for `count_ones`, or `umask` for
  `mode`. This feeds into the reasonable expectation that someone might search
  for the name and expect to find it ("what did Rust call `mkdir`").
- The same alias may be used for multiple declarations when the standard library
  provides multiple interfaces to the same function (for example,
  `slice::copy_from_slice` and `ptr::copy_nonoverlapping` are both aliased as
  `memcpy`). The same alias for `const` and non-`const` versions of the same
  function is also fine. We will not, however, add an alias for a function
  that's only somewhat similar or related.
- The alias must not conflict with the actual name of any existing declaration.
- As a special case for stdarch, aliases from exact assembly instruction names
  to the corresponding intrinsic function are welcome, as long as they don't
  conflict with other names.
