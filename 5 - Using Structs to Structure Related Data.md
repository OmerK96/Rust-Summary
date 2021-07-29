# Using Structs to Structure Related Data
## Defining and Instantiating Structs
An example struct:
```rust
struct User {
	username: String,
	email: String,
	sign_in_count: u64,
	active: bool,
}
```

An example struct instance (Note that the fields don't have to be in the same order as the struct itself):
```rust
let user1 = User {
	email: String::from("someone@example.com"),
	username: String::from("someusername123"),
	active: true,
	sign_in_count: 1,
};
```

Rust doesn't allow us to mark only certain fields of a struct as mutable, the entire instance must be mutable.  

The field init shorthand syntax allows us to write struct initialization a little shorter, so when a variable name is the same as a field name, you can omit the field specification:
```rust
fn build_user(email: String, username: String) -> User {
	User {
		email,
		username,
		active: true.
		sign_in_count: 1,
	}
}
```

Struct update syntax allows us to create a new instance from an old one, with minor changes:
```rust
let user2 = User {
	email: String::from("another@example.com"),
	username: String::from("anotherusername567"),
	..user1
};
```

You can also define structs that look similar to tuple, called tuple structs. Useful when you want to give the whole tuple a name and make the tuple be a different type from other tuples, and naming each field as in a regular struct would be verbose or redundant. Example:
```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);
 
let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```

You can also define structs that don't have any fields, they are called unit-like structs. They are useful when you need to implement a trait on some type but don't have any data that you want to store in the type itself.

## Method Syntax
Example method:
```rust
struct Rectangle {
	width: u32,
	height: u32,
}
 
impl Rectangle {
	fn area(&self) -> u32 {
		self.width * self.height
	}
}
 
fn main() {
	let rect1 = Rectangle { width:30, height: 50 };
	println!("The are of the rectangle is {} square pixels.", rect1.area());
}
```

Optional ways to refer to self:
* `self` - takes ownership
* `&self` - immutable reference
* `&mut self` - mutable reference

Rust doesn't have an equivalent to the `->` operator of the C language. Instead, Rust has a feature called automatic referencing and dereferencing. When you call a method with `object.something()`, Rust automatically adds in `&`, `&mut`, or `*` so `object` matches the signature of the method. For example, the following are the same:
```rust
p1.distance(&p2);
(&p1).distance(&p2);
```

Associated Functions are functions inside `impl` blocks that don't take self as a parameter. They are often used for constructors that will return a new instance of the struct. For example:
```rust
impl Rectangle {
	fn square(size: u32) -> Rectangle {
		Rectangle {width: size, height: size }
	}
}
```
