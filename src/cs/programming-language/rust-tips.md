ng# Rust Notes: A Developer's Collection of Rust Insights and Practices

## Introduction to Rust

Rust is a systems programming language focused on safety, speed, and concurrency. It offers powerful features like ownership, which provides memory safety without needing a garbage collector. Rust is an excellent choice for performance-critical applications.

## Installation

To install Rust on Linux or macOS, run the following command:

```bash
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

## Getting Started with Rust

### Creating a New Project with Cargo

Cargo is Rust's package manager and build system. To create a new Rust project:

```shell
cargo new hello_cargo
```

This command creates a new directory called `hello_cargo` with a basic Rust project structure.

### Writing Your First Rust Program: "Hello, World!"

Here's a simple "Hello, World!" program in Rust:

```rust
fn main() {
    println!("Hello, world!");
}
```

To run the code using Cargo:

```shell
cargo run
```

### First Program: Guess a Number

This example demonstrates basic user input handling in Rust:

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

### Generating a Secret Number

The following program generates a random number using the `rand` crate:

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

### Comparing the Guess to the Secret Number

This program combines input handling, random number generation, and conditional logic:

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

## Variable Types in Rust

### Print Variable Type in Rust
```rust
fn type_of<T>(_: T) -> &'static str {
    std::any::type_name::<T>()
}

// or 

std::mem::size_of::<usize>()
```

### Mutable and Immutable Variables

Variables in Rust are immutable by default. To make a variable mutable, use the `mut` keyword:

```rust
let mut x = 5;
x = 6;  // This is allowed because `x` is mutable
```

### Scalar Types

Rust provides several primitive scalar types:

- **Integer Types**: `i8`, `i16`, `i32`, `i64`, `i128`, `isize`, `u8`, `u16`, `u32`, `u64`, `u128`, `usize`
- **Floating-Point Types**: `f32`, `f64`
- **Boolean Type**: `bool`
- **Character Type**: `char`

### Compound Types

- **Tuples**: A tuple is a fixed-size group of values with different types:

    ```rust
    let tup: (i32, f64, u8) = (500, 6.4, 1);
    let (x, y, z) = tup;  // Destructuring assignment
    ```

- **Arrays**: Arrays are fixed-size collections of elements of the same type:

    ```rust
    let a = [1, 2, 3, 4, 5];
    let first = a[0];
    ```

## Functions in Rust

Functions are defined using the `fn` keyword. Here's a simple function that takes a parameter and returns a value:

```rust
fn plus_one(x: i32) -> i32 {
    x + 1
}
```

## Control Flow

### Conditional Statements

- Use `if` for conditional logic:

    ```rust
    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
    ```

### Loops

- **Infinite Loops**: `loop` is Rust's infinite loop:

    ```rust
    loop {
        println!("again!");
    }
    ```

- **`while` Loop**: Conditional loops:

    ```rust
    while number != 0 {
        println!("{number}!");
        number -= 1;
    }
    ```

- **`for` Loop**: Iterate over a collection:

    ```rust
    let a = [10, 20, 30, 40, 50];
    for element in a {
        println!("the value is: {element}");
    }
    ```

## Ownership in Rust

Ownership is Rust's unique approach to memory management without a garbage collector.

### Rules of Ownership

1. Each value in Rust has an owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.

### Borrowing and References

- References in Rust allow you to refer to some value without taking ownership:

    ```rust
    let s1 = String::from("hello");
    let len = calculate_length(&s1);  // Pass by reference

    fn calculate_length(s: &String) -> usize {
        s.len()
    }
    ```

## Collections

### Vectors

Vectors (`Vec<T>`) are resizable arrays:

```rust
let mut v: Vec<i32> = Vec::new();
v.push(1);
v.push(2);
```

### HashMaps

HashMaps store key-value pairs:

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

## Debugging
To get more information about the error, you can run your program with the `RUST_BACKTRACE=1` environment variable, as suggested by the error message. This will display a backtrace that can help you understand the sequence of events leading up to the error.


## Project Organization
### Comparison of **Package**, **Crate**, and **Module**

| **Concept**  | **Description**                                                                                                                                                                                                                                      |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Package**  | A **package** is a bundle that contains one or more **crates**. It is defined by a **`Cargo.toml` file** and managed by Cargo. A package must contain at least one crate.                                                                               |
| **Crate**    | A **crate** is the smallest **compilable unit** in Rust. It can either be a **binary crate** (which produces an executable) or a **library crate** (which produces reusable code). A package can have multiple binary crates and at most one library crate. |
| **Module**   | A **module** is a way to **organize and group code** within a crate. It is defined with the `mod` keyword and can be inline in the same file or split into separate files. Modules allow you to control visibility and structure code inside a crate.     |


e.g.:
```shell
my_project/
├── Cargo.toml                 # Defines the package (the project metadata)
└── src/
    ├── lib.rs                 # Library crate root (contains modules)
    ├── main.rs                # Binary crate root (the executable)
    ├── utils.rs               # A module used in the library crate
    ├── math/                  # A directory containing modules
    │   ├── mod.rs             # Root of the `math` module
    │   └── operations.rs      # A submodule of `math`
    └── services/
        ├── mod.rs             # Root of the `services` module
        └── network.rs         # A submodule of `services`
```


## References

- [The Rust Programming Language Book](https://doc.rust-lang.org/book/)
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/)

---