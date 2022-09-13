# See output of `build.rs`

If you are developing an application with a build script (`build.rs`), you might notice that anything printed to standard out is hidden from the user when running the build script. This is because cargo intentionally hides the output to keep it clean for the user.

However, you might want to see the output, for example when debugging the script. Luckily the output is all written to a file in the target area e.g. `target/debug/build/<pkg>/output`. Note that the path will be different if the target is on a different architecture e.g. `target/thumbv7em-none-eabi/debug/build/<pkg>/output`.

# References
- https://doc.rust-lang.org/cargo/reference/build-scripts.html#outputs-of-the-build-script
