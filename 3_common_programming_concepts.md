# Common Programming Concepts
## Variables and Mutability
Defining variables:
```rust
let x = 5; //immutable
let mut y = 7; //mutable
```

Immutable variables can be overwritten by using *shadowing*:
```rust
let x = 5;
x = x + 1; // not legit
let x = x + 1; // legit
```

Also allows to change the type of the variable:
```rust
let spaces = "   ";
let spaces = spaces.len();
```

## Data Types
2 types - scalar and compound.  
Scalar types are as follows:  
* Integer
* Floating point
* Boolean
* Character

### Integer Types
| Length  | Signed  | Unsigned | 
|---------|---------|----------|
| 8-bit   | `i8`    | `u8`     |
| 16-bit  | `i16`   | `u16`    |
| 32-bit  | `i32`   | `u32`    |
| 64-bit  | `i64`   | `u64`    |
| 128-bit | `i128`  | `u128`   |
| arch    | `isize` | `usize`  |

### Ways to write integers:
| Number literals  | Example       |
|------------------|---------------|
| Decimal          | `98_222`      |
| Hex              | `0xff`        |
| Octal            | `0o77`        |
| Binary           | `0b1111_0000` |
| Byte (`u8` only) | `b'A'`        |

### Floating Point Types
* `f32`
* `f64`

### Boolean Type - `bool`
* true
* false

### Character Type
* Size of char is *four bytes*, and represents a Unicode Scalar Value.
* Values range from `U+0000` to `U+D7FF` and `U+E000` to `U+10FFFF`.

### Compound Types - Tuples
* `let tup: (i32, f64, u8) = (500, 6.4, 1);`
* Supports destructuring: `let (x, y, z) = tup;`
* Supports accessing values with indexing: `let five_hundred = x.0;`

### Compound Types - Arrays
* Same type, constant length.
* `let a = [1, 2, 3, 4, 5];`
* Specifing type: `let a: [i32; 5] = [1, 2, 3, 4, 5]`
* Filling an array with values: `let a [3; 5] // 5 elements of 3`
* Accessing values by normal indexing: `let first = a[0];`

## Functions
Example function:
```rust
fn function(x: i32, y: i32) -> i32 {
    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
    x + y // NOTE: no semicolon in the end here
}
```

## Control Flow
### `if`
```rust
let number = 6;

if number % 4 == 0 {
    println!("number is divisible by 4");
} else if number % 3 == 0 {
    println!("number is divisible by 3");
} else {
    println!("number is not divisible by 4 or 3");
}
```

Conditions must be a `bool`. The following won't work:
```rust
let number = 3;
if number {
    println!("number was three");
}
```

It is possible to combine `if` in a `let` statement. All of the arms of the condition must have the same type of values.
```rust
let condition = true;
let number = if condition {
    5
} else {
    6
}; // NOTE: need a semicolon here
```

### `loop`
```rust
loop {
    println!("forever");
}
```

Loops can return values by using the `break` expression:
```rust
let mut counter = 0;
let result = loop {
    counter +=1;

    if counter == 10 {
        break counter * 2;
    }
}; // NOTE: need a semicolon here
```

### `while`
```rust
let mut number = 3;
while number != 0 {
    println!("{}!", number);
    number -= 1;
}
```

### `for`
For loops, to go through a collection:
```rust
let a = [10, 20, 30, 40, 50];
for element in a.iter() {
    println!("the value is: {}", element);
}
```
