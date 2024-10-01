# Mastering Comuter Systems

## 1 Operating Systems
Operating systems (OS) are software that manage computer hardware and software resources and provide common services for computer programs. Examples include Windows, macOS, and Linux.

### 1.1 Basic Concepts
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

##### Heap

##### Special Scenarios
Closure

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
