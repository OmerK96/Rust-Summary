# Generic Types, Traits, and Lifetimes
## Generic Data Types

Some examples for generics in rust:
* Functions:
```rust
fn example<T>(list: &[T]) -> &T {
    ...
}
```

* Structs:
```rust
struct Point<T> {
    x: T,
    y: T,
}
```

* Enums:
```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

* Methods:
```rust
impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}
```
Note that the `<T>` after `impl` means we're implementing methods on the type `Point<T>`, which is a generic type. We can implement it also on a specific type if we want to:
```rust
impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}
```

## Traits: Defining Shared Behavior
Trait definitions are a way to group method signatures together to define a set of behaviors necessary to accomplish some purpose.

For example:
```rust
pub trait Summary {
    fn summarize(&self) -> String;
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
```

One restriction to note with trait implementations is that we can implement a trait on a type only if either the trait or the type is local to our crate. For example, we can implement standard library traits like `Display` on a custom type like `Tweet` as part of our crate functionality.  
But we can’t implement external traits on external types. For example, we can’t implement the `Display` trait on `Vec<T>` within our crate, because `Display` and `Vec<T>` are defined in the standard library and aren’t local to our crate.

We can have default implementations to methods:
```rust
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}
```

We can have traits as parameters to functions:
```rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```

A note about traits as parameters: the above mentioned `impl` syntax is sugar syntax for trait bounds, which can be written as:
```rust
pub fn notify<T: Summary>(item: &T) {
```
It is mostly the same and shorter sometimes, but there are differences, like in the following example: The first function allows the parameters to have 2 different types, while the second does not:
```rust
pub fn notify(item1: &impl Summary, item2: &impl Summary) {

pub fn notify<T: Summary>(item1: &T, item2: &T) {
```

You can implement multiple traits like so:
```rust
pub fn notify(item: &(impl Summary + Display)) {

pub fn notify<T: Summary + Display>(item: &T) {
```

There is also a cleaner syntax for it: the `where` clause:
```rust
fn some_function<T, U>(t: &T, u: &U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{
```

# Validating References with Lifetimes
The syntax for annotating lifetimes is:
```rust
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```

Example function with lifetimes:
```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

Example struct with lifetimes:
```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}
```
