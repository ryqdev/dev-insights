# Golang Notes: A Developer’s Collection of Go Insights and Practices

## Introduction to Go

Go (or Golang) is a statically typed, compiled programming language designed for simplicity, concurrency, and efficiency. Developed by Google, Go is known for its clean syntax, robust standard library, and built-in support for concurrent programming.

## Getting Started

### Setting Up a Go Module

To create a new Go module, which is a collection of related Go packages:

```shell
go mod init <module path>
```

This command initializes a new Go module in the current directory and creates a `go.mod` file.


### Understanding `go mod why`

The `go mod why` command helps you understand why a particular module is included in your build. It shows the import path from your code to the module in question.

To use it, run:

```shell
go mod why <module>
```

For example, to find out why the `golang.org/x/text` module is included:

```shell
go mod why golang.org/x/text
```

This command will output the chain of imports that led to the inclusion of the specified module.

### `go install` vs `go get`
Starting in Go 1.17, installing executables with go get is deprecated. go install may be used instead.
- go install: install a command at a version specified on the command line while ignoring the go.mod file in the current directory
- go get: update dependencies in go.mod
For more information, access Deprecation of 'go get' for installing executables - The Go Programming Language

### Basic Structure of a Go Program

Every Go program is composed of packages. The `main` package is the entry point of any executable program.

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, world")
}
```

`func main()` is the entry point of the program.

## Go Basics

### Packages and Imports

- All Go code belongs to a package. The `main` package is for executable programs, while other packages are for libraries.
- You can import multiple packages:

```go
import (
    "fmt"
    "math"
)
```

### Exported Names

- A name is exported if it begins with a capital letter. For example, `math.Pi` is exported, but `math.pi` is not.

### Variables and Constants

- Variables are declared using `var`, and constants are declared using `const`.

```go
var i int
const Pi = 3.14
```

- Use `:=` for short variable declaration within functions:

```go
x := 10
```

### Basic Types

Go's basic types include:

- `int`, `uint`, `float32`, `float64`
- `string`
- `bool`
- `byte` (alias for `uint8`)
- `rune` (alias for `int32`, represents a Unicode code point)

### Type Conversion and Inference

- Type conversion is explicit:

```go
i := 42
f := float64(i)
```

- Go can infer types based on the initializer:

```go
i := 42           // int
f := 3.142        // float64
```

## Control Structures

### Conditional Statements

- `if` statements:

```go
if x < 0 {
    x = 0
} else if x > 10 {
    x = 10
}
```

- You can initialize a variable in the `if` statement:

```go
if v := math.Pow(x, n); v < lim {
    return v
}
```

### Loops

- Go has only one looping construct: the `for` loop.

```go
for i := 0; i < 10; i++ {
    sum += i
}
```

- `for` is also used as `while`:

```go
for sum < 1000 {
    sum += sum
}
```

### Switch Statements

- Use `switch` to execute code based on conditions:

```go
switch os := runtime.GOOS; os {
case "darwin":
    fmt.Println("OS X.")
case "linux":
    fmt.Println("Linux.")
default:
    fmt.Printf("%s.\n", os)
}
```

## Functions

- Functions in Go can return multiple results:

```go
func swap(x, y string) (string, string) {
    return y, x
}
```

- Named return values:

```go
func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return
}
```

## Error Handling

- Error handling in Go is done using the `error` type:

```go
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("cannot divide by zero")
    }
    return a / b, nil
}
```

## Data Structures

### Arrays

- Arrays have a fixed size and are value types:

```go
var a [2]string
a[0] = "Hello"
a[1] = "World"
```

### Slices

- Slices are dynamically-sized, more flexible than arrays:

```go
primes := []int{2, 3, 5, 7, 11, 13}
```

- Creating slices using `make`:

```go
s := make([]int, 0, 5) // len=0, cap=5
```

### Maps

- Maps are key-value pairs and are unordered:

```go
m := make(map[string]int)
m["age"] = 30
```

### Structs

- Structs are typed collections of fields:

```go
type Vertex struct {
    X, Y int
}
```

## Methods and Interfaces

### Methods

- Go allows you to define methods on types:

```go
type Vertex struct {
    X, Y float64
}

func (v Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

### Interfaces

- Interfaces define a set of method signatures:

```go
type Shape interface {
    Area() float64
}
```

## Concurrency in Go

### Goroutines

- A goroutine is a lightweight thread:

```go
go func() {
    fmt.Println("Hello from goroutine")
}()
```

### Channels

- Channels provide a way for two goroutines to communicate:

```go
ch := make(chan int)
ch <- 1    // send
v := <-ch  // receive
```

### Select Statement

- Use `select` to wait on multiple channels:

```go
select {
case v := <-ch1:
    fmt.Println(v)
case v := <-ch2:
    fmt.Println(v)
default:
    fmt.Println("No data received")
}
```

## Advanced Topics
### Assembly Go
Print the Assembly code for Golang
```shell
go tool compile -N -l -S main.go
```

### Reflection

- Reflection is used to inspect types at runtime using the `reflect` package.

### Generics

- Generics allow functions to operate on parameters of any type:

```go
func Print[T any](s []T) {
    for _, v := range s {
        fmt.Println(v)
    }
}
```

## Error Handling and Logging

- Use `defer` for resource management:

```go
f, _ := os.Open(filename)
defer f.Close()
```

### Panic and Recover

- Use `panic` and `recover` for handling unexpected errors:

```go
defer func() {
    if err := recover(); err != nil {
        fmt.Println("Recovered from:", err)
    }
}()
```

## Testing

- Use the built-in `testing` package to write unit tests:

```go
func TestAdd(t *testing.T) {
    if add(2, 2) != 4 {
        t.Error("Expected 2 + 2 to equal 4")
    }
}
```

Run tests with:

```shell
go test
```

## References

- [Go Official Documentation](https://golang.org/doc/)
- [Go by Example](https://gobyexample.com/)
- [Effective Go](https://golang.org/doc/effective_go)

---
