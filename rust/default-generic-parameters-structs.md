# Default generic parameters for structs

Rust allows you to specify defaults when defining a generic parameter. This can be useful so that the user may not have to specify the generic themself. For example, the user might usually want to use a u64 in a struct and avoid typing it out when instantiating it. You'd imagine it to work something like this:

```rust
struct A<T = u64>(T);

impl<T: Default> A<T> {
    fn new() -> Self {
        Self(Default::default())
    }
}

fn main() {
    let _a1 = A::new(); // This is equivalent to the next line
    let _a2 = A::<u64>::new();
}
```
> Note: when we say default generic parameter, it's completely different than the trait Default used to illustrate this example. Sorry for any confusion.

However, this doesn't compile! Instead you get an error message like this:
```
error[E0283]: type annotations needed for `A<T>`
  --> src/main.rs:10:15
   |
10 |     let _a1 = A::new(); // This is equivalent to the next line
   |         ---   ^^^^^^ cannot infer type for type parameter `T`
   |         |
   |         consider giving `_a1` the explicit type `A<T>`, where the type parameter `T` is specified
```

It appears that default generic parameters are fairly limited in their current form. Not specifying any parameter doesn't mean that the default is used - instead it means that type inference takes over. It's apparently equivalent to writing `let _a1 = A::<_>::new();` Since there's no hints, type inference fails.

To make use of the default parameter you have to use this odd looking syntax to qualify the type: `<A>::new()`. This is fairly unintuitive for the user so it seems these default generic parameters are not very useful at the moment.

```rust
struct A<T = u64>(T);

impl<T: Default> A<T> {
    fn new() -> Self {
        Self(Default::default())
    }
}

fn main() {
    let _a1 = <A>::new(); // This is the only line that changed
    let _a2 = A::<u64>::new();
}
```

# References
- https://www.reddit.com/r/rust/comments/ek6w5g/default_generic_type_inference/
- https://www.reddit.com/r/rust/comments/azqo9c/hey_rustaceans_got_an_easy_question_ask_here/eicgce8/
- https://stackoverflow.com/questions/37748306/default-generic-type-parameter-cannot-be-inferred
