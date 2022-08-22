# Enable feature on sub-dependency

Rust uses features to conditionally include code in a crate. You can enable features for dependencies in your project by specifying them in your `Cargo.toml` e.g.:

```toml
# Enables the `derive` feature of serde.
serde = { version = "1.0.118", features = ["derive"] }
```

However, what if you want to enable a feature on a sub-dependency i.e. a dependency of your crate's dependencies? There is no way to specify this directly in `Cargo.toml`. One apparently common use case for this is enabling a vendored build for OpenSSL (this builds OpenSSL from source and thus statically links it rather than the default behavior of dynamic linking which can be a pain for distributing the target binary).

However, you can just add the sub-dependency as a dependency and enable the feature there. Since Rust solves the dependency list to only compile each dependency once, you need this new dependency to be compatible with the sub-dependency's selected versions (note that features are *supposed* to be additive so you don't need to worry about matching those). The easiest way to do this is to just make the version `"*"` which matches any versions used in the sub-dependencies without adding additional restrictions (you could also just copy the version used in the sub-dependency but then if it ever gets updated you need to update your file... ain't nobody got time for that).

```toml
[dependencies]
openssl = { version = "*", features = ["vendored"] }
```

In addition, if you want to expose this ability to the users of your crate, or make it controllable from the build flags, you can "forward" the feature. Mark the sub-dependency as optional, which means that dependency line (including the feature) isn't active by default. Then define a new feature which enables the feature you want. The user can then control whether the dependency's feature through the new one.

```toml
# Your crate
[features]
static_ssl = ['openssl/vendored']

[dependencies]
openssl = { version = "*", features = ["vendored"], optional = true }
```

```toml
# The user's crate
[dependencies]
your-crate = { version = "1.2.3", features = ["static_ssl"] }
```

```sh
# Build your crate from the command line
$ cargo build --features openssl/vendored
```

# References
- https://stackoverflow.com/questions/59994525/how-do-i-specify-features-for-a-sub-dependency-in-cargo
- https://stackoverflow.com/questions/54775076/how-to-cross-compile-a-rust-project-with-openssl
- https://doc.rust-lang.org/cargo/reference/features.html
