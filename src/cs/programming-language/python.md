# Essential Python Programming Guide: Conventions, Data Structures, OOP, and Best Practices

## Naming Conventions in PEP8

Python follows specific naming conventions as outlined in PEP8. There are three main types:

1. **`lower_underscore`**: Used for variables, functions, and modules.
2. **`UPPER_UNDERSCORE`**: Used for constants.
3. **`CamelCase`**: Used for class definitions.

### 1. `lower_underscore`
- Applied to most identifiers: variables, functions, modules.

### 2. `UPPER_UNDERSCORE`
- Used for constants, which should not change after initialization.

### 3. `CamelCase`
- Used for naming classes.

## Underscore Prefix in Python

Python uses underscore prefixes to indicate the visibility of variables and methods.

### 1. Single Underscore (`_var`)
- Indicates a "weak private" variable. It’s a hint to the programmer not to use it outside of the class or module.

### 2. Double Underscore (`__var`)
- Indicates a "strong private" variable, name-mangled to prevent conflicts in subclasses.

## Type Annotations in Python

Using type hints improves code readability and helps with error detection.

- If a function does not return a value, use `-> None`:

```python
def foo() -> None:
    print("hello")
```

- For error-handling functions that never return, use `NoReturn`:

```python
from typing import NoReturn

def error() -> NoReturn:
    raise ValueError("An error occurred")
```

## Python List Slicing

Python supports slicing for accessing elements in sequences like lists, strings, and tuples.

Given a 2D array (matrix) using NumPy:

```python
import numpy as np
a = np.arange(9).reshape(3, 3)
```

`a` will look like:

```python
array([[0, 1, 2],
       [3, 4, 5],
       [6, 7, 8]])
```

Some slicing examples:

- `a[1]`: Returns the second row `[3, 4, 5]`.
- `a[1, :]`: Returns the entire second row `[3, 4, 5]`.
- `a[1, 2]`: Returns the element `5`.
- `a[:, 1]`: Returns the second column `[1, 4, 7]`.
- `a[1:, 1]`: Returns sub-array starting from the second row `[4, 7]`.
- `a[:2, 1]`: Returns sub-array containing first two rows `[1, 4]`.
- `a[:, 0:2]`: Returns all rows for the first two columns.

## Data Structures in Python

### 1. Stack

Stacks can be implemented using lists:

```python
stack = [1, 2, 3, 4, 5]
stack.append(6)
stack.pop()
```

### 2. Queue

Queues are better implemented using `collections.deque` for efficient operations:

```python
from collections import deque

queue = deque([1, 2, 3, 4, 5])
queue.append(6)
queue.popleft()
```

### 3. Set

Python has a built-in `set` type:

```python
my_set = {1, 2, 3, 4, 1, 2}
my_set.add(7)
print(9 in my_set)
```

### 4. Map (Dictionary)

Dictionaries in Python are used as maps:

```python
my_map = {'a': 1, 'b': 2, 'c': 3}
del my_map['a']
```

### 5. Linked List

Linked lists need to be implemented manually:

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

### 6. Tree

Tree nodes can be defined similarly:

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

### 7. Priority Queue (Heap)

Heaps are available via `heapq`:

```python
from heapq import heappush, heappop

heap = []
heappush(heap, (10, 'A'))
heappush(heap, (2, 'B'))
heappop(heap)
```

## `__name__` and Module Execution in Python

Python uses the `__name__` variable to determine if the module is being run directly or imported:

- `foo.py`:

    ```python
    def foo():
        print("in foo")
        print(__name__)

    if __name__ == "__main__":
        foo()
    ```

- `bar.py`:

    ```python
    from foo import foo

    def bar():
        print("in bar")
        print(__name__)

    if __name__ == "__main__":
        foo()
        bar()
    ```

## Object-Oriented Programming in Python

### Class Definition and Instantiation

```python
class MyClass:
    foo = 11

    def bar(self):
        return 'hello object'

instance = MyClass()
instance.bar()
```

### Class and Instance Variables

Class variables are shared among all instances, while instance variables are unique to each instance:

```python
class MyClass:
    addMe = 1

    def __init__(self, a, b):
        self.foo1 = a
        self.foo2 = b
```

## Debugging Python

### Command Line

Use `ipdb` for debugging:

```bash
python -m ipdb [file_name]
```

### Visual Studio Code

Configure `launch.json` for debugging:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal"
        }
    ]
}
```

## Performance Optimization

List comprehensions are faster than traditional loops:

```python
# Method 1
res = []
for i in range(1000000):
    if i % 2 == 0:
        res.append(i)

# Method 2 (Faster)
res = [x for x in range(1000000) if x % 2 == 0]
```

## String Methods: `join` and `split`

### `split`

```python
date = "2021-10-05".split('-')
```

### `join`

```python
date = '/'.join(['2021', '10', '05'])
```

## Argument Parsing in Python

Use `argparse` for more robust argument parsing in scripts:

```python
import argparse

if __name__ == '__main__':
    parser = argparse.ArgumentParser(description="A simple parser.")
    parser.add_argument("-p", "--morning", action="store_true")
    args = parser.parse_args()
```

## Copying Lists in Python

Python supports several methods to copy lists:

- **Assignment**: `new_list = old_list`
- **Shallow Copy**: `new_list = old_list.copy()`
- **Deep Copy**: `new_list = copy.deepcopy(old_list)`
- **Slicing**: `new_list = old_list[:]`

## Multiprocessing in Python

For parallel processing, use the `multiprocessing` module:

```python
import multiprocessing

def worker(i):
    print(f"Worker {i} is running")

if __name__ == '__main__':
    for i in range(3):
        p = multiprocessing.Process(target=worker, args=(i,))
        p.start()
```