# Python

## Name Conversion in PEP8

There are only 3 types in Python:
- lower_underscore = 0
- UPPER_UNDERSCORE = 1 
- CamelCase = 2

## UPPER_UNDERSCORE
const variables

## CamelCase
Class definition

## lower_underscore
Others, including variables, functions, modules


## Prefix Underscore
### prefix with one underscore
To hint the variable is private, and it is `weak private`

### prefix with two underscore
`strong private` 

## Typed Python
If the function has no return, using -> None in return is recommended

```python
def foo() -> None:
	print("hello")

```

For error handling functions, use NoReturn
```python
def error() -> NoReturn:
	raise ValueError
```

## python list slice

#### Some Cases:

As for 2-d list, such as matrix:

```python
import numpy as np
a=np.arange(9).reshape(3,3)
```

a :

```python
array([[0, 1, 2],
       [3, 4, 5],
       [6, 7, 8]])
```

a\[1]:

```python
array([3, 4, 5])
```

a\[1,:]:

```python
array([3, 4, 5])
```

a\[1,2]:

```
5
```

a\[:,1]:

```python
array([1, 4, 7])
```

a\[1:,1]:

```python
array([4, 7])
```

a\[:2,1]:

```python
array([1, 4])
```

a\[:,0:2];

```python
array([[0, 1],
       [3, 4],
       [6, 7]])
```

## Data Structure in Python

### Stack

Using lists as stacks

```python
stack = [1,2,3,4,5]
print("Init:", stack)
stack.append(6)
print("After push:", stack)
stack.pop()
stack.pop()
print("After pop:", stack)
```

### Queue

Though lists can be used to implement queue, it is slow when insert or remove the element at the front of queue. Therefore, `collections.deque` is recommended.

```python
from collections import deque
queue = deque([1,2,3,4,5])
print("Init:", queue)
queue.append(6)
print("Insert at the tail:", queue)
queue.appendleft(-1)
print("Insert at the head:", queue)
queue.pop()
print("Remove at the tail:", queue)
queue.popleft()
print("Remove at the head:", queue)
```

### Set

Python originally includes a data type for `sets`.

```python
mySet = {1,2,3,4,1,2}
print(mySet)
mySet.add(7)
print(9 in mySet)
```

### Map

Similiar with set, Python uses `dictionary` to implement map.

```python
myMap = {'a':1, 'b':2, 'c': 3}
print(myMap)
# {'a': 1, 'b': 2, 'c': 3}
del myMap['a']
print(myMap)
# {'b': 2, 'c': 3}
```

### Linked List

Like C++, linked list should be defined first.

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
    # this function is for printing
    def __str__(self):
        return str(self.val)

# create nodes
head = ListNode(0)
node1 = ListNode(1)
node2 = ListNode(2)
node3 = ListNode(3)

# link nodes
head.next = node1
node1.next = node2
node2.next = node3

# print
while head:
    print(head)
    head = head.next
```

### Tree

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
    def __str__(self):
        return str(self.val)

root = TreeNode('root')
node1 = TreeNode(1)
node2 = TreeNode(2)
root.left = node1
node1.right = node2

print(root.left.right)
```

### Priority Queue (Heap)

```python
from headq import heappush, heappop
myHeap = []
heappush(myHeap, (10,'A'))
heappush(myHeap, (2,'B'))
heappush(myHeap, (7,'C'))
print(myHeap)
heappop()
print(myHeap)

#output
# [(2, 'B'), (10, 'A'), (7, 'C')]
# [(7, 'C'), (10, 'A')]
```

## `__name__` in python

```python
# in foo.py
def foo():
    print("in foo")
    print(__name__)

if __name__ == "__main__":
    foo()

# output:
# in foo
# __main__
```

```python
# in bar.py
from foo import foo

def bar():
    print("in bar")
    print(__name__)

if __name__ == "__main__":
    foo()
    bar()

# output:
#in foo
#foo
#in bar
#__main__
```

## Object-Oriented Programming in Python

### Class Definition

```python
class MyClass:
    foo = 11
    def bar(self):
        return 'hello object'
```

`MyClass.foo` and `MyClass.bar` are valid attribute references, returning an **integer** and a **function** object, respectively

#### Class instantiation

```python
class MyClass:
    foo = 11
    def bar(self):
        return 'hello object'

instant = MyClass()
instant.bar() # call bar function
```

However, Many classes like to create objects with instances customized to a specific initial state. Therefore a class may define a special method named [`__init__()`](https://docs.python.org/3/reference/datamodel.html#object.\_\_init\_\_), like this:

```python
class MyClass:
    foo = 11
    def __init__(self, a, b):
      self.foo1 = a
      self.foo2 = b
    def bar(self):
        return 'hello object'

instant = MyClass(2,4)
print(instant.foo1, instant.foo2) #2 4
```

### Class and Instance Variables

Class variable shared by all instances

```python
class MyClass:
    addMe = 1
    def __init__(self, a, b):
      self.foo1 = a
      self.foo2 = b
    def bar(self):
        return 'hello object'
    def add(self,x):
      self.addMe += x

a = MyClass(2,4)
b = MyClass(3,6)
print(a.addMe)
a.add(2)
print(a.addMe)
b.add(3)
print(a.addMe)
print(b.addMe)

#1
#3
#3
#4
```

But sometimes it may lead to mistakes.(why)

```python
class MyClass:
    addMe = []
    def __init__(self, a, b):
      self.foo1 = a
      self.foo2 = b
    def bar(self):
        return 'hello object'
    def add(self,x):
      self.addMe.append(x)

a = MyClass(2,4)
b = MyClass(3,6)
print(a.addMe)
a.add(2)
print(a.addMe)
b.add(3)
print(a.addMe)

#[]
#[2]
#[2, 3]hon
```

### Reference

[9. Classes](https://docs.python.org/3/tutorial/classes.html)

## Debug Python

### Approach 1 (Command Line)

Like gdb interface.

```python
# remember to pip install ipdb before
python -m ipdb [file name]
```

### Approach 2 (VsCode)

In `launch.json` file:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "args": [
                "-a", "arg1",
                "-b" ,"arg2",
                "-c", "arg3"
            ]
        }
    ]
}
```

## for loop optimize in python

In general, list loop (I don't know the exact name of that method :P ) performs better than traditional way.

e.g

```python
import time
len = 10000000
start_time = time.time()

# method 1
res = []
for i in range(len):
    if i % 2 == 0:
        res.append(i)

checkPoint= time.time()
print("Execute: %s seconds" % (checkPoint - start_time))

# method 2
res = [x for x in range(len) if x % 2 == 0]

Terminate = time.time()
print("Execute: %s seconds" % (Terminate - checkPoint))

# output:
# Execute: 1.291942834854126 seconds
# Execute: 0.8715779781341553 seconds
```

List loop (method 2) is fast due to parallel computing.

## join and split method in python

### split

```python
import datetime
date = str(datetime.date.today())
print(date)
#2021-10-05
date = date.split('-')
print(date)
#['2021', '10', '05']
```

### join

```python
date = ['2021', '10', '05']
date = '/'.join(date)
print(date)
#2021/10/05
```

### Combine together

```python
import datetime
date = ''.join(str(datetime.date.today()).split('-'))
print(date)
#20211005
```

## python args

One of the useful ways to get the command line arguments for python is to use `parser`.

```python
from optparse import OptionParser
if __name__ == '__main__':
    parser = OptionParser()
    parser.add_option("-p", dest="morning", action="store_true", default=False)
    parser.add_option("-a", dest="afternoon", action="store_true", default=False)
    parser.add_option("-e", dest="evening", action="store_true", default=False)
    options = parser.defaults
    book = Booker(options['morning'], options['afternoon'], options['evening'])
    book.start()
```

## python copy list

When you want to create a new list based on an existing list, there are many situations

### Assignment

```python
oldList = [1,2,3,4,5]
newList = oldList
oldList.append(6)
print(oldList)
print(newList)
newList.append(7)
print(oldList)
print(newList)
```

### Copy

The copy() method only implements the deep copy of the first level, i.e., if there is a list in the list, the internal list will be assigned rather than copied.

```python
oldList = [1,2,[3,4],[5,6]]
newList = oldList.copy()
print(oldList)
print(newList)
oldList.append(7)
print(oldList)
print(newList)
newList.append(18)
print(oldList)
print(newList)
oldList[2].append(188)
print(oldList)
print(newList)
```

### Deep copy

Everything is copied

```python
import copy
oldList = [1,2,[3,4],[5,6]]
newList = copy.deepcopy(oldList)
oldList.append(7)
print(oldList)
print(newList)
newList.append(18)
print(oldList)
print(newList)
oldList[2].append(188)
print(oldList)
print(newList)
```

### Slice

the same with `copy()` method

```python
import copy
oldList = [1,2,[3,4],[5,6]]
newList = oldList[:]
# omitted
```

### Initializaiton with for loop

the same with `copy()` method

```python
import copy
oldList = [1,2,[3,4],[5,6]]
newList = [i for i in oldList]
# omitted
```

## Multiprocess in Python

```python
import os
import multiprocessing

def foo(i):
    print("arg: ", i)
    print("Hi this is ", multiprocessing.current_process().name)
    print('module name:', __name__)
    print('parent process id:', os.getppid())
    print('current process id:', os.getpid())
    print('------------------------')
if __name__ == '__main__':
    for i in range(3):
        p = multiprocessing.Process(target=foo, args=(i,))
        p.start()
```