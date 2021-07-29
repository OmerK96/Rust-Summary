# Enums and Pattern Matching
## Defining an Enum
```rust
Example enum:
enum IpAddr {
	V4(u8, u8, u8, u8),
	V6(String),
}

// Accessing an enum
let home = IpAddr::V4(127,0,0,1);
let loopback = IpAddr::V6(String::from("::1"));
```

Like structs, you can define methods on enums using `impl` blocks.  

In Rust, an enum called `Option<T>` is used as a replacement for `NULL`, and that the value of type `T` may or may not be valid.
```rust
enum Option<T> {
	Some(T),
	None,
}
```

Examples:
```rust
let some_number = Some(5);
let some_string = Some("a string");
// Must specify type - compiler can't infer from None
let absent_number: Option<i32> = None;
```

The main difference in the two approaches, is that `Option<T>` and `T` are different types, hence the compiler won't let us use an `Option<T>` value as if it were definitely a valid value. For example, the following code won't compile:
```rust
let x: i8 = 5;
let y: Option<i8> = Some(5);
let sum = x + y; // DOES NOT COMPILE!!!
```

## The `match` Control Flow Operator
`match` is a control flow operator that allows us to compare a value against a series of patterns and then execute code based on which pattern matches.  
The value of the match expression is the value of the match arm that its pattern matched.
```rust
enum Coin {
	Penny,
	Nickel,
	Dime,
	Quarter,
}
 
fn value_in_cents(coin: Coin) -> u8 {
	match coin {
		Coin::Penny => {
			println!("Lucky penny!");
			1
		},
		Coin::Nickel => 5,
		Coin:: Dime => 10,
		Coin::Quarter => 25,
	}
}
```

`match` arms can also bind to the parts of the values that match the pattern. In that way, we can extract values out of enum variants:
```rust
#[derive(Debug)]
enum UsState {
	Alabama,
	Alaska,
	// --snip--
}
 
enum Coin {
	Penny,
	Nickel,
	Dime,
	Quarter(UsState),
}
 
fn value_in_cents(coin: Coin) -> u8 {
	match coin {
		Coin::Penny => 1,
		Coin::Nickel => 5,
		Coin:: Dime => 10,
		Coin::Quarter(state) => {
			println!("State quarter from {:?}!", state);
			25
		},
	}
}
```

Using a match statement is the primary way to extract the `T` value from an `Option<T>`.  

Matches are exhaustive, meaning we need to consider every possible case (For example, we have to both use `Some<T>` and `None` when matching with an `Option<T>`). In case we want to ignore some patterns, we can write the special pattern `_` instead:
```rust
// Reminder: this is a way to specify that 0 is of type u8
let some_u8_value = 0u8;
match some_u8_value {
	1 => println!("one"),
	3 => println!("three"),
	5 => println!("five"),
	7 => println!("seven"),
	_ => (),
}
```

In the previous example, the `()` is just the unit value, so nothing will happen in the `_` case.

## Concise Control Flow with `if let`
When we only care about one value in an `Option<T>`, we can use the following syntax instead of writing a `match` statement:
```rust
if let Some(3) = some_u8_value {
	println!("three");
}
```

Note that there is only 1 equal sign!  
You can also add an `else` with an `if let`:
```rust
let mut count = 0;
if let Coin::Quarter(state) = coin {
	println!("State quarter from {:?}!", state);
} else {
	count += 1;
}
```
