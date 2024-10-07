https://www.gnu.org/software/make/manual/make.html

## Variable Assignment

In GNU Make, variable assignment is a key concept for controlling how and when variables are defined and used in your Makefile. Here’s a guide to the different types of variable assignment in Make:

### Types of Variable Assignment

1. **Simple Assignment (`=`)**

   This is the most common type of variable assignment. When you use `=`, the value is evaluated only when the variable is used (lazy evaluation).

   ```makefile
   VAR = value
   ```

   Example:
   ```makefile
   VAR = $(shell echo Hello)
   all:
       @echo $(VAR)
   ```

   In this case, `$(VAR)` will expand to the result of the `shell` function, which is `Hello`.

2. **Immediate Assignment (`:=`)**

   With `:=`, the value is evaluated immediately when the assignment occurs (eager evaluation).

   ```makefile
   VAR := value
   ```

   Example:
   ```makefile
   VAR := $(shell echo Hello)
   all:
       @echo $(VAR)
   ```

   Here, `$(VAR)` will also be `Hello`, but the key difference is that the value of `VAR` is evaluated at the time of assignment, not when it's used.

3. **Conditional Assignment (`?=`)**

   This assigns a value to a variable only if it’s not already defined.

   ```makefile
   VAR ?= default_value
   ```

   Example:
   ```makefile
   VAR ?= default
   all:
       @echo $(VAR)
   ```

   If `VAR` was not previously defined, it will be assigned `default`. If `VAR` had a different value before, it will retain its original value.

4. **Append Assignment (`+=`)**

   This appends to the value of an existing variable.

   ```makefile
   VAR += more_text
   ```

   Example:
   ```makefile
   VAR = initial
   VAR += more
   all:
       @echo $(VAR)
   ```

   In this case, `$(VAR)` will be `initial more`.

### Example Makefile Using Various Assignments

```makefile
# Simple Assignment
VAR1 = $(shell echo "Simple")

# Immediate Assignment
VAR2 := $(shell echo "Immediate")

# Conditional Assignment
VAR3 ?= $(shell echo "Conditional")

# Append Assignment
VAR4 = Base
VAR4 += Appended

all:
    @echo VAR1: $(VAR1)
    @echo VAR2: $(VAR2)
    @echo VAR3: $(VAR3)
    @echo VAR4: $(VAR4)
```

### Summary

- **`=`**: Lazy evaluation, value expanded when used.
- **`:=`**: Eager evaluation, value expanded at assignment.
- **`?=`**: Assign value only if variable is not already defined.
- **`+=`**: Append to the current value of the variable.

Understanding these types of variable assignments helps in writing more efficient and predictable Makefiles, allowing you to control when and how variable values are computed and used.