# Breaking changes

Breaking changes should be avoided when possible. [RFC 1105] lays the foundations for what constitutes a breaking change. Breakage may be deemed acceptable or not based on its actual impact, which can be approximated with a [crater] run.

There are strategies for mitigating breakage depending on the impact.

For changes where the value is high and the impact is high too:

- Using compiler lints to try phase out broken behavior.

If the impact isn't too high:

- Looping in maintainers of broken crates and submitting PRs to fix them.

[crater]: ./crater.md
[RFC 1105]: https://rust-lang.github.io/rfcs/1105-api-evolution.html

## For reviewers

Look out for changes to documented behavior and new trait impls for existing stable traits.
