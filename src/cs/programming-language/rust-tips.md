# Rust 

## Installation

For Linux or macOS:
```bash
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

## Basic

### Hello World

Create projects with cargo
```shell
cargo new hello_cargo
```

The rust code:
```rust
fn main() {
    println!("Hello, world!");
}
```

To run the code, use cargo:

```shell
cargo run
```

### The first program: guess a number

```rust
use std::io;

fn main() {
    println!("Guess the number!");
    println!("Please input your guess.");

    let mut guess = String::new();
    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}");
}
```


### The second program: generate a secret number

```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");
    let secret_number = rand::thread_rng().gen_range(1..=100);
    println!("The secret number is: {secret_number}");
    println!("Please input your guess.");
    let mut guess = String::new();
    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");
    println!("You guessed: {guess}");
}

```

### The third program: compare the guess to the secret number

```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");
    let secret_number = rand::thread_rng().gen_range(1..=100);
    loop {
        println!("Please input your guess.");
        let mut guess = String::new();
        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");
        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };
        println!("You guessed: {guess}");
        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

### Variable types

#### Mutable and immutable

The variable in Rust is immutable by default. If we need a mutable variable, we should use `mut` keyword.

```rust
let mut x = 5;
```

#### Scalar Types

* Integer types

Integer Types in Rust:

| Length  | Signed  | Unsigned |
| ------- | ------- | -------- |
| 8-bit   | `i8`    | `u8`     |
| 16-bit  | `i16`   | `u16`    |
| 32-bit  | `i32`   | `u32`    |
| 64-bit  | `i64`   | `u64`    |
| 128-bit | `i128`  | `u128`   |
| arch    | `isize` | `usize`  |

Integer Literals in Rust:

| Number literals  | Example       |
| ---------------- | ------------- |
| Decimal          | `98_222`      |
| Hex              | `0xff`        |
| Octal            | `0o77`        |
| Binary           | `0b1111_0000` |
| Byte (`u8` only) | `b'A'`        |

#### Floating-Point Types

```rust
fn main() {
    let x = 2.0; // f64
    let y: f32 = 3.0; // f32
}
```

#### Boolean Types

```rust
fn main() {
    let t = true;
    let f: bool = false; // with explicit type annotation
}
```

#### Character Types

```rust
fn main() {
    let c = 'z';
    let z: char = 'ℤ'; // with explicit type annotation
    let heart_eyed_cat = '😻';
}
```

#### Compound Types

* Tuple type

```rust
fn main() {
    // case 1
    let x: (i32, f64, u8) = (500, 6.4, 1);
    let five_hundred = x.0;
    let six_point_four = x.1;
    let one = x.2;
    // case 2
    let tup = (500, 6.4, 1);
    let (a, b, c) = tup;
    println!("The value of y is: {y}");
}
```

#### Array Type

Arrays in Rust have a fixed length.

```rust
fn main() {
    // case 1
    let a = [1, 2, 3, 4, 5];
    let first = a[0];
    let second = a[1];
    
    // case 2
    let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
          
}
```

### Functions

```rust
fn main() {
    let x = plus_one(5);
    println!("The value of x is: {x}");
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```

#### Control flow

* If statement

```rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
    
    
    if number { // Error: number should be a bool type variable
        println!("number was three");
    }
}
```

* Repetition with loops

```rust
fn main() {
    // dead loop
    let mut x = 1;
    loop {
        println!("{x}");
        x += 1;
    }

    // return value from loops
    let mut counter = 0;
    let result = loop {
        counter += 1;
        if counter == 10 {
            break counter * 2;
        }
    };
    println!("The result is {result}");
    
    
    //conditional loop with while
    let mut number = 3;
    while number != 0 {
        println!("{number}!");
        number -= 1;
    }
    println!("LIFTOFF!!!");
     
    // loop through a collection with for
    let a = [10, 20, 30, 40, 50];
    for element in a {
        println!("the value is: {element}");
    }
}
```

### Ownership
_Ownership_ is a set of rules that govern how a Rust program manages memory

Ownership primarily deals with the following aspects of heap memory management:
- Efficient management of heap memory
- Prevention of memory operation issues, such as double-freeing

Borrowing Rules:
1. At any given time, you can have either one mutable reference or any number of immutable references.
2. References must always be valid

#### Ownership Rules

* Each value in Rust has an _owner_.
* There can only be one owner at a time.
* When the owner goes out of scope, the value will be dropped.


```rust
{                      // s is not valid here, it’s not yet declared
    let s = "hello";   // s is valid from this point forward
    // do stuff with s
}                      // this scope is now over, and s is no longer valid
```

### Collection

The most common collections are:

* Vector
* HashMap

#### Vector

Vector is one kind of *dynamic array.

#### HashMap

```rust
use std::collections::HashMap;
let mut my_map: HashMap<i32, i32> = HashMap::new();
​
my_map.insert(100, 1);
my_map.insert(200, 2);
my_map.insert(300, 3);
```


## References
[https://doc.rust-lang.org/book/title-page.html](https://doc.rust-lang.org/book/title-page.html)