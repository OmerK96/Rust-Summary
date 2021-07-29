# Getting Started
## Updating and Uninstalling
```sh
$ rustup update
$ rustup self uninstall
```

## Creating a Project with Cargo
```sh
$ cargo new new_name
$ cd new_name
```

## Example `cargo.toml` File
```
[package]
name = "new_name"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]

[dependencies]
```

## Cargo commands
* build - Just compiles
* run - Builds and runs the program
* check - Checks that there are no compilation errors, without compiling