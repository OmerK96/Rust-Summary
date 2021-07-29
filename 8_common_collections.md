# Common Collections
## Storing Lists of Values with Vectors
Creating a vector:
```rust
let v: Vec<i32> = Vec::new();
```

Macro to initialize a vector with values:
```rust
let v = vec![1, 2, 3];
```

Updating a vector:
```rust
let mut v = Vec::new();
v.push(5); //NOTE: we didn't need to specify the type annotation here because the compiler 
                   // inferred it from this operation
```

Note that dropping a vector drops its elements (Reminder: dropping means freeing).  

Reading from a vector: 2 options:
```rust
let third: i32 = &v[2];
match v.get(2) {
    Some(third) => println!("third is {}", third),
    None => println!("third element doesn't exist"),
}
```

the first option panics if the index doesn't exist, while the second one doesn't.

Iterating over a vector:
```rust
for i in &v {
    println!("{}", i);
}
```

Iteration with change to the members:
```rust
for i in &mut v {
    *i += 50;
}
```

## Storing UTF-8 Encoded Text with Strings
Several string types and slices:
* Normal: `String`, `&str`
* OS: `OsString`, `&OsStr`
* C: `CString`, `&CStr`

`String` means its owned, and `str` means its borrowed.

Creating a new `String`:
```rust
let mut s = String::new();
// OR
let data = "initial contents";
let s = data.to_string();
// OR
let s = String::from("initial contents");
```

We can update a string using the method `push_str`. Note that it takes a slice as an argument, so it won't take ownership of the passed string:
```rust
let mut s1 = String::from("foo");
let s2 = "bar";
s1.push_str(s2);
```

We can also use `push` like a normal vector.  
In the same logic as `push_str`, where we take a slice as the second argument, we can use the `+` operator, which calls the `add` method:
```rust
let s1 = String::from("Hello, ");
let s2 = String::from("World!");
let s3 = s1 + &s2;
```

Another method is with `format`:
```rust
let s = format!("{}-{}", s1, s2);
```

Finally, we can call `chars()` or `bytes()` to iterate over strings:
```rust
for c in "abc".chars() { … }
for b in "abc".bytes() { … }
```

## Storing Keys with Associated Values in Hash Maps
Creating a Hash Map (need to `use` the `HashMap` struct first):
```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

All of the keys must be of the same type, and all of the values must be of the same type.
When we put owned values (like `String`) in the hash map, the values are moved (unless they implement the `Copy` trait).

Accessing values in a hash map:
```rust
let team_name = String::from("Blue");
let score = scores.get(&team_name); // The result is an Option<&V> enum
```

Looping over elements in the hash map:
```rust
for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```

There are several ways to update a hash map:  
### Overwriting a value:
```rust
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Blue"), 25);
```

### Inserting only if entry doesn't exist:
```rust
let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.entry(String::from("Yellow")).or_insert(50); // Will insert
scores.entry(String::from("Blue")).or_insert(50); // Won't insert
```

### Updating a value based on the old one:
```rust
for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}
```

Note that the `on_insert` method returns a mutable reference to the value for the corresponding `Entry` key if that key exists, and if not, insert the parameter as the new value for this key and returns a mutable reference to the new value.