# Golang

## Basic

### The Structure of a Go File

Create a new module:

```shell
go mod init <module path>
```

### Hello World in Golang

```go
package main
​
import "fmt"
​
func main() {
  fmt.Println("Hello, world")
}
```

Func main() is the entry point of the program

### Printf

`%v` can match all kinds of variables while printing.

```go
fmt.Printf("%v", i)
fmt.Printf("%v", d)
fmt.Printf("%v", s)
fmt.Printf("%v", p)
fmt.Printf("%+v", p)
fmt.Printf("%#v", p)
fmt.Printf("%.2f", f)
```

### Packages

All our code must belong to a package

### Imports

Import other packages like this:

```go
// multiple lines
import (
  "fmt"
  "math"
)
​
// single line
import "fmt"
​
```

### Create third-party packages

1. Create a public GitHub repo like this [https://github.com/Yukun4119/golang_utils](https://github.com/Yukun4119/golang_utils)
2. Finish some functions and create a tag on GitHub
3. import that package
```shell
go get github.com/Yukun4119/golang_utils@vx.x.x
```

e.g.:
```go
package main

import "github.com/Yukun4119/golang_utils/example"

func main() {
	example.Hello()
}
```

### go get vs go install

```shell
get         #add dependencies to current module and install them
install     #compile and install packages and dependencies
```

### Exported names

In Go, a name is exported if it begins with a capital letter. For example, `Pizza` is an exported name, as is `Pi`, which is exported from the math package.

`pizza` and `pi` do not start with a capital letter, so they are not exported.

```go
// Here, we should use math.Pi instead of math.pi
func main() {
  fmt.Println(math.Pi)
}
```

### Functions

```go
func add(x int, y int) int {
  return x + y
}
​
// or use the shortened format
func add(x, y int) int {
  return x + y
}
```

A function in Go can return multiple results

```go
func swap(x, y string) (string, string) {
  return y, x
}
```

Go's return values may be named. If so, they are treated as variables defined at the top of the function. A return statement without arguments returns the named return values. This is known as a "naked" return.

```go
func split(sum int) (x, y int) {
  x = sum * 4 / 9
  y = sum - x
  return
}
```

### Variables

The 'var' statement declares a list of variables; as in function argument lists, the type is last.

```go
var c, python, java bool
var i int

// If an initializer is present, the type can be omitted; the variable will take the type of the initializer.
var c, python, java = true, false, "no!"
var i, j int = 1, 2

// Inside a function, the := short assignment statement can be used in place of a var declaration with implicit type.
k := 3
c, python, java := true, false, "no!"
```

### Basic types

Go's basic types are

```shell
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```

### Zero values

* `0` for numeric types,
* `false` for the boolean type, and
* `""` (the empty string) for strings.

### Type conversions

The expression T(v) converts the value v to the type T.

```go
i := 42
f := float64(i)
u := uint(f)
```

### Type inference

```go
var i int
j := i // j is an int

i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

### Constants

Constants are declared like variables, but with the const keyword. Constants can be character, string, boolean, or numeric values. Constants cannot be declared using the := syntax.

```go
const World = "世界"
const Truth = true
```

### For Statement

```go
for i := 0; i < 10; i++ {
	sum += i
}

// The init and post statements are optional.
for ; sum < 1000; {
	sum += sum
}

// For is Go's "while"
for sum < 1000 {
	sum += sum
}

// dead loop
for {
}
```

### If Statement

```go
if x < 0 {
    x = 1
}

// the if statement can start with a short statement to execute before the condition.
if v := math.Pow(x, n); v < lim {
	return v
}

// if else
if v := math.Pow(x, n); v < lim {
	return v
} else {
	fmt.Printf("%g >= %g\n", v, lim)
}
```

### Switch Statement

```go
switch os := runtime.GOOS; os {
case "darwin":
	fmt.Println("OS X.")
case "linux":
	fmt.Println("Linux.")
default:
	// freebsd, openbsd,
	// plan9, windows...
	fmt.Printf("%s.\n", os)
}
```

### Defer

A defer statement defers the execution of a function until the surrounding function returns.

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

```go
defer fmt.Println("world")
fmt.Println("hello")
```

`defer` is often used when resources need to be freed in some way. For example when we open a file we need to make sure to close it later. With `defer`:

```go
f, _ := os.Open(filename)
defer f.Close()
```

This has 3 advantages:&#x20;

* it keeps our `Close` call near our `Open` call so it's easier to understand
* if our function had multiple return statements (perhaps one in an `if` and one in an `else`) `Close` will happen before both of them and&#x20;
* deferred functions are run even if a run-time panic occurs.

### Panic & Recover

```go
package main

import "fmt"

func main() {
	defer func() {
		if err := recover(); err != nil {
			logs.Error("FATAL err=%v", err)
		}
	}()

	panic("PANIC")
}
```

### Pointers

The type \*T is a pointer to a T value. Its zero value is nil.

```go
var p *int
```

The & operator generates a pointer to its operand.

```go
i := 42
p = &i
```

Unlike C, Go has no pointer arithmetic.

### Structs

```go
type Vertex struct {
	X int
	Y int
}

func main() {
    v := Vertex{1, 2}
	v.X = 4
	fmt.Println(v.X)

    // Pointers to structs
    p := &v
	p.X = 1e9
	fmt.Println(v)
}
```

### Arrays

In Go, arrays are value semantics. An array variable represents the entire array, it is not an implicit pointer to the first element (like C language arrays), but a complete value. When an array variable is assigned or passed, the entire array is actually copied.

A string is an immutable byte sequence, strings are usually used to contain human-readable text data. Unlike arrays, the elements of a string cannot be modified, it is a read-only byte array. Although the length of each string is also fixed, the length of the string is not part of the string type.

[]byte == string

```go
var a [2]string
a[0] = "Hello"
a[1] = "World"
fmt.Println(a[0], a[1])
fmt.Println(a)

primes := [6]int{2, 3, 5, 7, 11, 13}
fmt.Println(primes)
```

### Slices

A slice is formed by specifying two indices, a low and high bound, separated by a colon:

```go
a[low : high]
```

```go
primes := [6]int{2, 3, 5, 7, 11, 13}
var s []int = primes[1:4]
```

Slices are like references to arrays

```go
// A slice literal is like an array literal without the length.
q := []int{2, 3, 5, 7, 11, 13}
fmt.Println(q)

r := []bool{true, false, true, true, false, true}
fmt.Println(r)

s := []struct {
	i int
	b bool
}{
	{2, true},
	{3, false},
	{5, true},
	{7, true},
	{11, false},
	{13, true},
}
fmt.Println(s)
```

```go
// Slice length and capacity
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// Slice the slice to give it zero length.
	s = s[:0]
	printSlice(s)

	// Extend its length.
	s = s[:4]
	printSlice(s)

	// Drop its first two values.
	s = s[2:]
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

#### Creating a slice with make

```go
package main

import "fmt"

func main() {
	a := make([]int, 5)
	printSlice("a", a)

	b := make([]int, 0, 5)
	printSlice("b", b)

	c := b[:2]
	printSlice("c", c)

	d := c[2:5]
	printSlice("d", d)
}

func printSlice(s string, x []int) {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}
```

#### Appending to a slice

```go
var s []int
printSlice(s)

// append works on nil slices.
s = append(s, 0)
printSlice(s)
```

### Range Statement

```go
// The first is the index, and the second is a copy of the element at that index.
for i, v := range pow {
	fmt.Printf("2**%d = %d\n", i, v)
}

// You can skip the index or value by assigning to _.
for i, _ := range pow
for _, value := range pow

// If you only want the index, you can omit the second variable.
for i := range pow
```

### Maps

maps in go is unordered.

```go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}
```

```go
// Insert or update an element in map m:
m[key] = elem
// Delete an element:
delete(m, key)
// Test that a key is present with a two-value assignment:
elem, ok = m[key]
```

### Methods

Go does not have classes. However, you can define methods on types.

```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}
```

A method is just a function with a receiver argument. Therefore, the following code is equivalent

```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func Abs(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(Abs(v))
}
```

### Interfaces

An interface type is defined as a set of method signatures.

### Goroutines

A goroutine is a lightweight thread managed by the Go runtime.

```go
go f(x, y, z)
```

starts a new goroutine running

```go
f(x, y, z)
```

### Channels

Channels are a typed conduit through which you can send and receive values with the channel operator, `<-`.

```go
ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.
```

Like maps and slices, channels must be created before use:

```go
ch := make(chan int)
```

Channels can be _buffered_. Provide the buffer length as the second argument to `make` to initialize a buffered channel:

```go
ch := make(chan int, 100)
```

How to stop a Goroutine
```go
	cancel := make(chan struct{})
	go func() {
		for {
			select {
			case <-cancel:
				fmt.Println("cancel")
				return
			}
		}
	}()
    // ...
	cancel <- struct{}{}
```

### Select

The `select` statement lets a goroutine wait on multiple communication operations.

A `select` blocks until one of its cases can run, then it executes that case. It chooses one at random if multiple are ready.

**go get and go mod install**&#x20;

```shell
go get -u
```

is risky, avoid using it in most cases.



```shell
go get XXXXXXX@latest
```

It will update certain packages to the latest version.



```shell
go mod install
```

It will install packages according to the content of go.mod

### Reflect

#### What is reflect

Package reflect implements run-time reflection, allowing a program to manipulate objects with arbitrary types. The typical use is to take a value with static type interface{} and extract its dynamic type information by calling TypeOf, which returns a Type.

A call to ValueOf returns a Value representing the run-time data. Zero takes a Type and returns a Value representing a zero value for that type.

#### Why do we need reflect

```go
switch x := x.(type) {
    case stringer:
        return x.String()
    case string:
        return x
    case int:
        return strconv.Itoa(x)
    // ...similar cases for int16, uint32, and so on...
```

The cases will be countless...

We need a better method to handle type selector.

#### Reflect usage

#### Access value in run time

* reflect.Type

```go
// 函数 reflect.TypeOf 接受任意的 interface{} 类型，并以 reflect.Type 形式返回其动态类型
t := reflect.TypeOf(3)  // a reflect.Type
fmt.Println(t.String()) // "int"
fmt.Println(t)          // "int"

// These two lines are the same
// fmt.Printf 提供了一个缩写 %T 参数，内部使用 reflect.TypeOf 来输出
fmt.Printf("%T\n", i)
fmt.Println(reflect.TypeOf(i))

//因为 reflect.TypeOf 返回的是一个动态类型的接口值，它总是返回具体的类型。因此，下面的代码将打印 "*os.File" 而不是 "io.Writer"
var w io.Writer = os.Stdout
fmt.Println(reflect.TypeOf(w)) // "*os.File"
```

* 使用 reflect.Value 的 Kind 方法来替代之前的类型 switch

```go
package format

import (
    "reflect"
    "strconv"
)

// Any formats any value as a string.
func Any(value interface{}) string {
    return formatAtom(reflect.ValueOf(value))
}

// formatAtom formats a value without inspecting its internal structure.
func formatAtom(v reflect.Value) string {
    switch v.Kind() {
    case reflect.Invalid:
        return "invalid"
    case reflect.Int, reflect.Int8, reflect.Int16,
        reflect.Int32, reflect.Int64:
        return strconv.FormatInt(v.Int(), 10)
    case reflect.Uint, reflect.Uint8, reflect.Uint16,
        reflect.Uint32, reflect.Uint64, reflect.Uintptr:
        return strconv.FormatUint(v.Uint(), 10)
    // ...floating-point and complex cases omitted for brevity...
    case reflect.Bool:
        return strconv.FormatBool(v.Bool())
    case reflect.String:
        return strconv.Quote(v.String())
    case reflect.Chan, reflect.Func, reflect.Ptr, reflect.Slice, reflect.Map:
        return v.Type().String() + " 0x" +
            strconv.FormatUint(uint64(v.Pointer()), 16)
    default: // reflect.Array, reflect.Struct, reflect.Interface
        return v.Type().String() + " value"
    }
}
```

* display with recursion

```go
func display(path string, v reflect.Value) {
    switch v.Kind() {
    case reflect.Invalid:
        fmt.Printf("%s = invalid\n", path)
    case reflect.Slice, reflect.Array:
        for i := 0; i < v.Len(); i++ {
            display(fmt.Sprintf("%s[%d]", path, i), v.Index(i))
        }
    case reflect.Struct:
        for i := 0; i < v.NumField(); i++ {
            fieldPath := fmt.Sprintf("%s.%s", path, v.Type().Field(i).Name)
            display(fieldPath, v.Field(i))
        }
    case reflect.Map:
        for _, key := range v.MapKeys() {
            display(fmt.Sprintf("%s[%s]", path,
                formatAtom(key)), v.MapIndex(key))
        }
    case reflect.Ptr:
        if v.IsNil() {
            fmt.Printf("%s = nil\n", path)
        } else {
            display(fmt.Sprintf("(*%s)", path), v.Elem())
        }
    case reflect.Interface:
        if v.IsNil() {
            fmt.Printf("%s = nil\n", path)
        } else {
            fmt.Printf("%s.type = %s\n", path, v.Elem().Type())
            display(path+".value", v.Elem())
        }
    default: // basic types, channels, funcs
        fmt.Printf("%s = %s\n", path, formatAtom(v))
    }
}
```

#### Example: encode

```go
func encode(buf *bytes.Buffer, v reflect.Value) error {
    switch v.Kind() {
    case reflect.Invalid:
        buf.WriteString("nil")

    case reflect.Int, reflect.Int8, reflect.Int16,
        reflect.Int32, reflect.Int64:
        fmt.Fprintf(buf, "%d", v.Int())

    case reflect.Uint, reflect.Uint8, reflect.Uint16,
        reflect.Uint32, reflect.Uint64, reflect.Uintptr:
        fmt.Fprintf(buf, "%d", v.Uint())

    case reflect.String:
        fmt.Fprintf(buf, "%q", v.String())

    case reflect.Ptr:
        return encode(buf, v.Elem())

    case reflect.Array, reflect.Slice: // (value ...)
        buf.WriteByte('(')
        for i := 0; i < v.Len(); i++ {
            if i > 0 {
                buf.WriteByte(' ')
            }
            if err := encode(buf, v.Index(i)); err != nil {
                return err
            }
        }
        buf.WriteByte(')')

    case reflect.Struct: // ((name value) ...)
        buf.WriteByte('(')
        for i := 0; i < v.NumField(); i++ {
            if i > 0 {
                buf.WriteByte(' ')
            }
            fmt.Fprintf(buf, "(%s ", v.Type().Field(i).Name)
            if err := encode(buf, v.Field(i)); err != nil {
                return err
            }
            buf.WriteByte(')')
        }
        buf.WriteByte(')')

    case reflect.Map: // ((key value) ...)
        buf.WriteByte('(')
        for i, key := range v.MapKeys() {
            if i > 0 {
                buf.WriteByte(' ')
            }
            buf.WriteByte('(')
            if err := encode(buf, key); err != nil {
                return err
            }
            buf.WriteByte(' ')
            if err := encode(buf, v.MapIndex(key)); err != nil {
                return err
            }
            buf.WriteByte(')')
        }
        buf.WriteByte(')')

    default: // float, complex, bool, chan, func, interface
        return fmt.Errorf("unsupported type: %s", v.Type())
    }
    return nil
}

// Marshal encodes a Go value in S-expression form.
func Marshal(v interface{}) ([]byte, error) {
    var buf bytes.Buffer
    if err := encode(&buf, reflect.ValueOf(v)); err != nil {
        return nil, err
    }
    return buf.Bytes(), nil
}
```

#### Modify variables in run time

* What is variable?

一个变量就是一个可寻址的内存空间，里面存储了一个值，并且存储的值可以通过内存地址来更新

```go
x := 2                   // value   type    variable?
a := reflect.ValueOf(2)  // 2       int     no
b := reflect.ValueOf(x)  // 2       int     no
c := reflect.ValueOf(&x) // &x      *int    no
d := c.Elem()            // 2       int     yes (x)

// 我们可以通过调用reflect.Value的CanAddr方法来判断其是否可以被取地址
fmt.Println(a.CanAddr()) // "false"
fmt.Println(b.CanAddr()) // "false"
fmt.Println(c.CanAddr()) // "false"
fmt.Println(d.CanAddr()) // "true"
```

我们可以通过调用reflect.ValueOf(\&x).Elem()，来获取任意变量x对应的可取地址的Value。

```go
x := 2
d := reflect.ValueOf(&x).Elem()   // d refers to the variable x
d.Set(reflect.ValueOf(4))
fmt.Println(x) // "4"
```

Calling the Set method on a non-addressable `reflect.Value` will also cause panic, but this error will not be detected by the compiler, and panic will be reported when it is run.

```go
x := 2
b := reflect.ValueOf(x)
b.Set(reflect.ValueOf(3)) // panic: Set using unaddressable value
```

#### A Word of Caution

* Code based on reflection is quite fragile. For every issue that would cause the compiler to report a type error, there is a corresponding misuse in reflection. The difference is that the compiler will report the error immediately during build, while reflection will throw a panic exception when it is actually run, which may be long after the code is written, and the program may have been running for a long time.
  * The best way to avoid this fragility caused by reflection is to keep all reflection-related usage within the package. If possible, avoid exposing the reflect.Value type directly in the package's API, which can limit some illegal inputs.
  * If this is not possible, perform extra type checks before each risky operation. For example, when fmt.Printf receives an illegal operand, it does not throw a panic exception, but prints relevant error information. Although the program still has bugs, it is easier to diagnose.
* Reflection also reduces the security of the program and affects the accuracy of automated refactoring and analysis tools because they cannot recognize type information that can only be confirmed at runtime.
* Code based on reflection usually runs one to two orders of magnitude slower than normal code. For a typical project, the performance of most functions is not closely related to the overall performance of the program, so consider using reflection when it can make the program clearer. Testing is a particularly suitable scenario for using reflection, because each test has a small data set. However, for performance-critical path functions, it is best to avoid using reflection.



### Goroutines

```go
package main

import (
	"fmt"
	"time"
)

func foo(x int) {
	fmt.Println(x + 1)
}

func main() {
	go func(msg string) {
		fmt.Println(msg)
	}("hello")

	go foo(7)

	time.Sleep(time.Second)
}

```

### Generics

```go
func Print[T any](s []T) {
	for _, v := range s {
		fmt.Println(v)
	}
}

func main() {
	Print[int]([]int{1, 2, 3})
	Print[float64]([]float64{1.1, 2.1, 3.3})
}
```

### Customed Triangular Arbitrage with Generics

```go
func IfThenElse[T any](condition bool, trueValue, falseValue T) T {
    if condition {
        return trueValue
    }
    return falseValue
}
```

### Closures
```go
package main

import "fmt"

func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-i),
		)
	}
}
```


### Empty struct

```go
type Msg struct {
	Value int
}

func main() {
	m := &Msg{}
	if reflect.DeepEqual(m, &Msg{}) {
		fmt.Println(1)
	}
	m.Value = 1
	fmt.Println(m)
}
```

## Reference
[https://pkg.go.dev/reflect](https://pkg.go.dev/reflect)
[https://www.golang-book.com/books/intro](https://www.golang-book.com/books/intro)