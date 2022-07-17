# Use README in docs

If you're going to the effort of writing nice documentation for a crate, you might find that you are duplicating information between your README.md and your inline crate documentation. You can avoid this duplication by directly using the contents of the README file in your docs.

The simplest way to document your code is using the `//! ` and `/// ` line prefixes. This is automatically parsed by `cargo doc` and included in the crate documentation. But, these line prefixes are just syntactic sugar for the `#[doc]` attribute. For example, `/// Helpful description` is equivalent to `#[doc = "Helpful description"]`.

The `include_str!()` macro can also be used inside inside this attribute to pull the contents of a file into the documentation. To include your README.md file as module documentation, use `#![doc = include_str!("../README.md")]`. Note that `#![doc]` documents the thing this attribute is inside of (usually the module itself) whlie `#[doc]` documents the thing after the attribute.

Obviously, this idea works in other situations as well - anytime you have text in a separate file you want to use in your docs.


# References
- https://doc.rust-lang.org/rustdoc/write-documentation/the-doc-attribute.html
- https://blog.guillaume-gomez.fr/articles/2021-08-03+Improvements+for+%23%5Bdoc%5D+attributes+in+Rust
- https://github.com/rust-lang/rust/issues/37901
