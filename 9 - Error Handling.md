# Error Handling
## Unrecoverable Errors with `panic!`
the `panic!` macro is used when some error happened and there is nothing you can do about it. It will print a failure message, unwind and clean up the stack, and then quit.
```rust
fn main() {
    panic!("crash and burn");
}
```

## Recoverable Errors with `Result`
The basic form of handling a `Result` and panicking when an error occurs:
```rust
let f = File::open("hello.txt");

let f = match f {
    Ok(file) => file,
    Err(error) => panic!("Problem opening the file: {:?}", error),
};
```

There is a shortcut for that: `unwrap` and `expect`, the latter is like the former but allows us to pass a custom error message:
```rust
let f = File::open("hello.txt").unwrap();
let f = File::open("hello.txt").expect("Failed to open hello.txt");
```

We can also propagate the errors and let the caller handle the error. The shortcut for that is:
```rust
let f = File::open("hello.txt")?;
```