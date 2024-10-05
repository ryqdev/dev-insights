# Mastering Linux System

## Operating Systems
Operating systems (OS) are software that manage computer hardware and software resources and provide common services for computer programs. Examples include Windows, macOS, and Linux.

This document is for **Linux** system.

### Basic Concepts
#### Memory Management
##### Memory Layout
```shell
|-------------------------------|  Highest Memory Address
|           Stack               |  
|  (Grows Downwards)            |  <-- Grows downwards
| Local variables,              |
| Function calls                |
|-------------------------------|  
|           Unused Space        | 
|-------------------------------|
|           Heap                |
|  (Dynamically Grows Upwards)  |
| Dynamically allocated         |  <-- Grows upwards 
| memory (e.g., malloc,         |
| new in C, Box in Rust)        |
|-------------------------------|  
|      Uninitialized Data (BSS) |  <-- Uninitialized global/static variables
|-------------------------------|
|      Initialized Data         |  <-- Initialized global/static variables
|-------------------------------|
|       Text (Code) Segment     |  <-- Stores program code (machine instructions)
|  (Executable Code - Read Only)|
|-------------------------------|  Lowest Memory Address (0x0000)

```
##### Stack 


The **stack** is a general-purpose data structure used for storing data temporarily, following the last-in, first-out (LIFO) principle. It is a region of memory that grows and shrinks dynamically as functions are called and return.

###### Example (Stack as a Whole)

```shell
High Address
|-------------------------------|
| Stack Frame: function C        | <-- Most recent function call
|-------------------------------|
| Stack Frame: function B        | <-- Function that called C
|-------------------------------|
| Stack Frame: function A        | <-- Function that called B
Low Address
```

In Unix/Linux, you can check the stack size using the `ulimit` command:
```shell
ulimit -s  # Shows the stack size in kilobytes (KB)
```

A **stack frame** (also known as an activation record) is a portion of the stack that contains all the information relevant to a single function call.

- Each time a function is called, a new stack frame is created. This frame holds:
    - Function parameters (if passed on the stack).
    - Local variables for the function.
    - Return address to know where to return after the function finishes.
    - Saved registers (like the base pointer or other CPU registers).

```shell
High Address
|-------------------------------|
| Return Address (to caller)     |
|-------------------------------|
| Function Parameter: b          |
|-------------------------------|
| Function Parameter: a          |
|-------------------------------|
| Local Variable: result = a + b |
Low Address
```

## Key Components of a Stack Frame

1. **Function Parameters**:
        - These are the arguments passed to the function when it is called. They are stored in the frame so the function can use them during execution.

2. **Local Variables**:
        - These are variables declared within the function. They are stored in the stack frame so that the function has access to them throughout its execution.

3. **Return Address**:
        - This is the address in the calling function where control should return after the called function finishes executing. This ensures the program continues execution at the right place.

4. **Saved Registers** (optional, depending on the architecture):
        - If the function modifies important registers (like the instruction pointer or base pointer), the current state of those registers may be saved in the frame so that they can be restored when the function returns.

5. **Return Value** (in some cases):
        - In some languages or architectures, the return value of the function may also be stored in the frame before it is returned to the calling function.

## Example Breakdown of a Stack Frame

Consider this simple C code example:

```c
int add(int a, int b) {
         int result = a + b;
         return result;
}

int main() {
         int x = 5, y = 10;
         int sum = add(x, y);
         return 0;
}
```

When `add(x, y)` is called from `main()`, a new **stack frame** for `add()` is created. This frame will contain:

- **Function parameters (`a = 5`, `b = 10`)**: The values of `x` and `y` from `main()` are passed into `add()` and stored in its stack frame as the parameters `a` and `b`.
- **Local variable (`result`)**: The local variable `result` is allocated space in the frame.
- **Return address**: The address to which the program will return when `add()` finishes (this would point to the next instruction in `main()` after the call to `add()`).

### Initial State (when `main()` starts)

```shell
High Address
| Return Address (OS)             |
| Local Variables: x = 5, y = 10  |
Low Address
```

### After Calling `add()`

```shell
High Address
| Return Address (OS)             |
| Local Variables: x = 5, y = 10  |
|------------------------------- |
| Return Address (main())         |
| Function Parameters: a = 5, b = 10 |
| Local Variables: result         |
Low Address
```

## Stack Frame Lifecycle

1. **Function Call**: When a function is called, a new stack frame is created. This frame holds all necessary information (like local variables and parameters) for the function's execution.
2. **Execution**: The function operates using the values stored in its frame.
3. **Return**: Once the function completes, the stack frame is popped off the stack, and the return address is used to continue execution at the correct point in the calling function.

## In Recursive Functions

In recursive functions, each recursive call generates a new stack frame, even though the function is the same. This is why recursion can sometimes cause **stack overflow** if the recursion depth is too large — each recursive call requires a new stack frame, and the stack has a limited size.

### Example

```c
int factorial(int n) {
        if (n == 0)
            return 1;
        return n * factorial(n - 1);
}

int main() {
        int result = factorial(5);
        return 0;
}
```

Each call to `factorial()` will create a new frame on the stack. When `factorial(5)` calls `factorial(4)`, a new frame is created, and so on until the base case is reached.

#### Recursive Calls Stack

```shell
| Stack Frame: factorial(0)      | <-- base case
| Stack Frame: factorial(1)      |
| Stack Frame: factorial(2)      |
| Stack Frame: factorial(3)      |
| Stack Frame: factorial(4)      |
| Stack Frame: factorial(5)      |
| Stack Frame: main()            |
```

Once the base case is reached, the frames start to be popped off, and the return values propagate up the call chain.

###### Register-based Parameter Passing

In many modern systems, particularly on **64-bit architectures** like **x86-64**, the first few function arguments are passed using **registers**, rather than the stack. This is done for performance reasons, as accessing data in registers is much faster than accessing data on the stack.

####  **System V AMD64 ABI (Linux/macOS 64-bit)**

- This is the standard calling convention on **Linux** and **macOS** for 64-bit systems.
- The first **six integer or pointer arguments** are passed via **registers**.

- **Registers used** for the first six arguments:
    - `rdi`: first integer argument (e.g., `a`).
    - `rsi`: second integer argument (e.g., `b`).
    - `rdx`: third integer argument.
    - `rcx`: fourth integer argument.
    - `r8` : fifth integer argument.
    - `r9` : sixth integer argument.

- Any remaining arguments (beyond six) are passed on the **stack**.


##### Heap
The **heap** is a region of memory used for **dynamic memory allocation**. Unlike the **stack**, which operates in a last-in, first-out (LIFO) fashion and is managed automatically by the system, the **heap** allows for **manual memory management**, where memory is allocated and deallocated at runtime by the programmer or the language's runtime system.

The heap is used to allocate large blocks of memory or memory whose lifetime extends beyond the scope of the function that created it.

###### Heap Characteristics:
- **Size**: The heap is generally much larger than the stack and can grow as needed (up to a limit defined by the system).
- **Access**: Memory on the heap is accessed through pointers or references.
- **Lifetime**: Memory on the heap persists until it is explicitly freed (manual memory management) or automatically reclaimed (automatic memory management).
- **Flexible Allocation**: The heap allows for dynamic memory allocation of arbitrary sizes during program execution.

###### How the Heap Organizes Memory

The heap is often managed by a **memory allocator** or **heap manager**, which is responsible for handling memory requests from the program. Here are the typical steps:

1. **Free List**:
   - The heap is often divided into a **free list** of available memory blocks. When a program requests memory, the allocator finds a suitable block in this list.
   - There are different strategies to find free blocks:
     - **First fit**: The allocator finds the first block large enough to satisfy the request.
     - **Best fit**: The allocator finds the smallest block that can satisfy the request, minimizing wasted space.
     - **Buddy Allocation**: The allocator splits blocks into powers of two to efficiently manage memory and minimize fragmentation.

2. **Splitting and Coalescing**:
   - When a block is allocated, it may be **split** into smaller blocks, leaving part of the block available for future allocations.
   - When memory is freed, adjacent free blocks are **coalesced** (merged) to form larger blocks, reducing fragmentation.

3. **Fragmentation**:
   - **External Fragmentation** occurs when free memory is scattered across the heap in small, non-contiguous blocks, making it difficult to allocate larger blocks of memory.
   - **Internal Fragmentation** occurs when the allocated memory block is larger than what is needed, wasting space within the allocated block.

###### How the Heap Manages Memory

The heap's memory management involves **allocating**, **deallocating**, and **organizing** memory blocks. Memory management on the heap can be manual or automatic, depending on the programming language.

1. **Manual Memory Management** (C/C++)

In languages like **C** and **C++**, the programmer is responsible for both allocating and freeing heap memory.

 **Allocating Memory**:
- In C/C++, memory is allocated dynamically from the heap using functions like `malloc()`, `calloc()`, or `realloc()`.

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int* array = (int*)malloc(10 * sizeof(int)); // Allocates space for 10 integers on the heap

    if (array == NULL) {
        printf("Memory allocation failed!\n");
        return 1;  // Exit if memory allocation failed
    }

    // Use the allocated memory
    for (int i = 0; i < 10; i++) {
        array[i] = i * 2;
    }

    // Print the array
    for (int i = 0; i < 10; i++) {
        printf("%d ", array[i]);
    }
    printf("\n");

    free(array); // Deallocates the memory pointed to by `ptr`

    return 0;
}

```

- Failure to deallocate memory can lead to **memory leaks**, where the heap memory is used up without being released, causing the program to run out of memory over time.

2. **Automatic Memory Management** (Garbage Collection)

In languages like **Java**, **Python**, **Go**, and **OCaml**, memory management on the heap is handled **automatically** by the language runtime through **garbage collection**. The programmer does not need to manually free memory.

**Allocation**:
- Memory is allocated on the heap using constructs like `new` (Java) or through dynamic object creation (e.g., Python objects).

```java
// Java example
int[] arr = new int[10];  // Allocates space for 10 integers on the heap
```

```python
# Python example
arr = [1, 2, 3, 4, 5]  # Dynamically allocates memory on the heap
```

##### **Garbage Collection**:
- The **garbage collector** automatically tracks memory usage and frees memory that is no longer referenced by the program.
- Garbage collection typically works by identifying objects that are no longer accessible from the **root set** (global variables, local variables, etc.). These objects are then marked for collection and their memory is freed.

There are different types of garbage collectors:
- **Mark and Sweep**: The garbage collector traverses all references, marks active objects, and sweeps away unmarked objects.
- **Generational GC**: Memory is divided into generations, with newer objects being collected more frequently.
- **Reference Counting**: Each object keeps a count of how many references point to it, and when the count drops to zero, the object is collected.

##### **Pros** of Garbage Collection:
- **No manual memory management**: The runtime automatically handles memory allocation and deallocation.
- **Reduced risk of memory leaks**: The garbage collector helps avoid memory leaks that occur due to manual memory management errors.

##### **Cons**:
- **Performance overhead**: Garbage collection adds overhead and can cause unpredictable pauses in program execution, especially in real-time systems.
- **Lack of control**: The programmer has less control over when memory is freed.

3. **Memory Safety Guarantee**

**C++ RAII (Resource Acquisition Is Initialization)**

**RAII** is a programming idiom used in **C++** for managing resources in runtime. In RAII, resources are tied to the lifetime of an object, meaning that when an object is created, it acquires necessary resources (such as memory, file handles, or network sockets), and when the object goes out of scope (i.e., is destroyed), it automatically releases those resources.

Example of RAII in C++:
```cpp
#include <iostream>
#include <memory>  // for std::unique_ptr

class Resource {
public:
    Resource() {
        std::cout << "Resource acquired\n";
    }
    ~Resource() {
        std::cout << "Resource released\n";
    }
};

void someFunction() {
    std::unique_ptr<Resource> res = std::make_unique<Resource>();
    // Resource is automatically released when `res` goes out of scope.
}

int main() {
    someFunction();
    return 0;
}
```

In this example:
- The **`Resource`** object is created on the heap using `std::unique_ptr` (a smart pointer).
- When the function `someFunction` exits, the `std::unique_ptr` goes out of scope, and its destructor is called, which automatically releases the heap-allocated resource.
- This eliminates the need to manually call `delete` and ensures that resources are cleaned up properly, even if an exception occurs.

###### RAII with `std::unique_ptr` and `std::shared_ptr`:
- **`std::unique_ptr`**: A smart pointer that guarantees **unique ownership** of the heap-allocated memory. When the pointer goes out of scope, it automatically deletes the object.
- **`std::shared_ptr`**: A smart pointer that allows **shared ownership** of a heap-allocated object. The memory is freed only when the last `std::shared_ptr` pointing to the object is destroyed.

###### Benefits of RAII:
- **Automatic Resource Management**: RAII ties resource management to object lifetimes, preventing resource leaks.
- **Exception Safety**: Resources are automatically released even in the presence of exceptions, preventing memory leaks and ensuring program correctness.

###### RAII vs Manual Memory Management:
- In **RAII**, the resource is acquired and released automatically as part of the object's lifecycle, avoiding manual `malloc/free` or `new/delete` operations.
- It’s a more elegant and safer approach than manual memory management since it minimizes the chance of memory leaks and dangling pointers.

###### **Memory Management in Rust**

**Rust** uses a **unique memory management system** that combines the advantages of **manual memory management** and **automatic memory management** through a concept called **ownership**. Rust eliminates the need for a garbage collector while guaranteeing **memory safety** at compile time.

###### Key Concepts in Rust's Memory Management:

1. **Ownership**:
   - Every value in Rust has a single **owner**, and when the owner goes out of scope, the memory is automatically deallocated.
   - There is no need for manual memory management (`malloc`/`free`) or garbage collection.

2. **Borrowing and References**:
   - Instead of copying or transferring ownership, Rust allows **borrowing** references (`&` for immutable, `&mut` for mutable references) to data without transferring ownership.
   - This ensures that data can be accessed safely by multiple parts of a program without risking memory leaks or invalid access.

3. **Lifetimes**:
   - Rust uses **lifetimes** to track how long references are valid, ensuring that references never outlive the data they point to. This is checked at compile time, so there are no dangling references or use-after-free errors.

###### Example of Ownership in Rust:
```rust
fn main() {
    let s1 = String::from("hello");  // s1 owns the heap-allocated string
    let s2 = s1;                     // Ownership of the string is moved to s2, s1 is now invalid

    // println!("{}", s1);           // This would cause a compile-time error, because s1 is no longer valid
    println!("{}", s2);              // s2 is valid and can be used
}
```

- **Ownership Transfer**: In Rust, when you assign `s1` to `s2`, **ownership** of the heap-allocated string is transferred from `s1` to `s2`. After this, `s1` is invalid and can no longer be used.
- **No Double-Free**: Since Rust enforces strict ownership rules, there is no risk of double-freeing memory or accessing invalid memory.

###### Example of Borrowing in Rust:
```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1); // Pass a reference to s1
    println!("The length of '{}' is {}.", s1, len); // s1 is still valid
}

fn calculate_length(s: &String) -> usize {
    s.len()  // Access the string through a reference
}
```

- **Borrowing**: In this case, `&s1` is an **immutable reference** to the string. The ownership of `s1` is not transferred, so it remains valid after the function call.
- Rust enforces **borrowing rules** at compile time to prevent race conditions and memory issues.

###### Rust’s Memory Safety Guarantees:
- **No Null Pointers**: Rust does not allow null pointers; instead, it uses the `Option<T>` type to represent values that may or may not exist.
- **No Dangling References**: The borrow checker ensures that references do not outlive the data they point to.
- **No Data Races**: Rust’s ownership and borrowing system ensures that no data races can occur in concurrent programs, making it a great choice for **safe concurrency**.

#### Examples

```c
#include <stdlib.h>
#include <stdio.h>

int main() {
    int *p1 = malloc(sizeof(int));

    *p1 = 42;

    int *p2 = p1;

    return 0;
}
```

The variable p1 itself is stored on the stack.

In the line int *p1 = malloc(sizeof(int));, p1 is a pointer variable that is declared on the stack. The malloc function allocates memory on the heap and returns a pointer to that memory location, which is then assigned to p1.

```shell
+---------------+
|  Stack  |
+---------------+
|  p1 (pointer)  |  Address: 0x7fffcf401230
|  (4 bytes)     |
+---------------+
|  p2 (pointer)  |  Address: 0x7fffcf401234
|  (4 bytes)     |
+---------------+



+---------------+
|  Heap  |
+---------------+
|  Integer value  |
|  (42)          |
+---------------+
```


## Distributed Systems
Distributed systems consist of multiple autonomous computers that communicate through a network. They work together to achieve a common goal and provide benefits such as resource sharing, scalability, and fault tolerance.

## Networking
Networking involves the interconnection of computers and other devices to share resources and information. Key concepts include protocols, IP addresses, and network topologies.

## Security
Security in computer science systems involves protecting information and systems from unauthorized access, attacks, and damage. It includes practices such as encryption, authentication, and intrusion detection.
