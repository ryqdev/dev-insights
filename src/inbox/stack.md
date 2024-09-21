# Understanding Stack Frames in Function Calls

## Overview

The **stack** is a general-purpose data structure used for storing data temporarily, following the last-in, first-out (LIFO) principle. It is a region of memory that grows and shrinks dynamically as functions are called and return.

### Example (Stack as a Whole)

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

### Visual Representation of the Stack and Stack Frames

```shell
Stack (entire structure) - Grows downward in memory
--------------------------------------------------------
| Stack Frame: function C                               |
|    Local Variables                                    |
|    Function Parameters                                |
|    Return Address                                     |
--------------------------------------------------------
| Stack Frame: function B                               |
|    Local Variables                                    |
|    Function Parameters                                |
|    Return Address                                     |
--------------------------------------------------------
| Stack Frame: function A                               |
|    Local Variables                                    |
|    Function Parameters                                |
|    Return Address                                     |
--------------------------------------------------------
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

## Register-based Parameter Passing

In many modern systems, particularly on **64-bit architectures** like **x86-64**, the first few function arguments are passed using **registers**, rather than the stack. This is done for performance reasons, as accessing data in registers is much faster than accessing data on the stack.

### Register-based Parameter Passing

In 64-bit architectures, function parameters are typically passed via registers to improve efficiency. Let's look at the two most common calling conventions on modern 64-bit systems:

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
