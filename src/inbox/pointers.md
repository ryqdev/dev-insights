# Pointers

## Raw Pointers

### Pointers in Golang
```go
package main

import (
	"fmt"
)

func main() {
	var x int = 1
	var ptr *int = &x
	fmt.Println(x)
	fmt.Println(ptr)
	fmt.Println(*ptr)

	x = 2
	fmt.Println(x)
	fmt.Println(ptr)
	fmt.Println(*ptr)

	*ptr = 3
	fmt.Println(x)
	fmt.Println(ptr)
	fmt.Println(*ptr)
}
// 1
// 0x14000110018
// 1
// 2
// 0x14000110018
// 2
// 3
// 0x14000110018
// 3
```


### Pointers in C

```c
#include <stdio.h>

int main() {
    int x = 1;
    int *p = &x;
    printf("%d\n", x);
    printf("0x%x\n", p);
    printf("%d\n", *p);

    x = 2;
    printf("%d\n", x);
    printf("0x%x\n", p);
    printf("%d\n", *p);

    *p = 3;
    printf("%d\n", x);
    printf("0x%x\n", p);
    printf("%d\n", *p);

    return 0;
}
// 1
// 0x6b25eb38
// 1
// 2
// 0x6b25eb38
// 2
// 3
// 0x6b25eb38
// 3
```


### Pointers in C++
```cpp
#include<iostream>

int main() {
    int x = 1;
    int *p = &x;
    std::cout << x << std::endl;
    std::cout << p << std::endl;
    std::cout << *p << std::endl;

    x = 2;
    std::cout << x << std::endl;
    std::cout << p << std::endl;
    std::cout << *p << std::endl;

    *p = 3;
    std::cout << x << std::endl;
    std::cout << p << std::endl;
    std::cout << *p << std::endl;

    return 0;
}
// 1
// 0x16d84eb38
// 1
// 2
// 0x16d84eb38
// 2
// 3
// 0x16d84eb38
// 3
```


### Pointers in Rust
```rust
fn main() {
    let mut x: i32 = 1;
    let p: *mut i32 = &mut x;
    println!("{x}");
    println!("{:?}",p);
    unsafe { println!("{}", *p); }

    x = 2;
    println!("{x}");
    println!("{:?}",p);
    unsafe { println!("{}", *p); }

    unsafe { *p = 3; }
    println!("{x}");
    println!("{:?}",p);
    unsafe { println!("{}", *p); }
}
// 1
// 0x16d6de1a4
// 1
// 2
// 0x16d6de1a4
// 2
// 3
// 0x16d6de1a4
// 3
```

## Struct Pointers

```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;

    // constructer
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    explicit TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
};

void printTreeVertically(const std::string& prefix, TreeNode* root, bool isLeft) {
    if (root != nullptr) {
        std::cout << prefix;
        std::cout << (isLeft ? "\\--" : "|--");
        std::cout << root -> val << std::endl;
        printTreeVertically(prefix + (isLeft ? "   " : "|  "),root -> right, false);
        printTreeVertically(prefix + (isLeft ? "   " : "|  "),root -> left, true);
    }
}
```

## Smart Pointers


## Reference
