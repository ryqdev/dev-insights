# C++ STL 源码解读

[https://github.com/arkingc/note/blob/master/C%2B%2B/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90.md#%E4%BA%8C%E7%A9%BA%E9%97%B4%E5%88%86%E9%85%8D%E5%99%A8](https://github.com/arkingc/note/blob/master/C%2B%2B/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90.md#%E4%BA%8C%E7%A9%BA%E9%97%B4%E5%88%86%E9%85%8D%E5%99%A8)

[https://github.com/arkingc/note/blob/master/C%2B%2B/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90.md](https://github.com/arkingc/note/blob/master/C%2B%2B/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90.md)

STL源码有若干个版本，本文主要是讲解SGI STL

#### vector

STL vector 原理及应用

初学C++的时候，便喜欢上用vector来代替一般的数组，因为vector不像一般数组那样死板，其长度是可变的，还有许多内置的方法，操作起来很方面。

### vector的内部机制

vector可以理解为，**动态分配**的元素数组。源代码中的定义如下：

```cpp
template<class _Ty,
    class _Ax>
    class vector
        : public _Vector_val<_Ty, _Ax>
    {   // varying size array of values
public:
    /********/
protected:
    pointer _Myfirst;   // pointer to beginning of array
    pointer _Mylast;    // pointer to current end of sequence
    pointer _Myend; // pointer to end of array
    };
```

vector是线性连续存储，用三个指针去管理该连续内存。具体的内部实现机制可以参考这篇文章[STL vector的内部实现原理及基本用法](https://blog.csdn.net/u012658346/article/details/50725933)

### emplace\_back VS push\_back

一般vector最常用的**插入**和**删除**方法是`push_back` 和`pop_back`。

但是我在刷leetcode的时候，发现某些大神是用`emplace_back`来代替`push_back`

要区分`emplace_back`和`push_back`的区别，就要先理清C++中`左值`和`右值`的区别

#### 左值和右值

* **左值**：lvalue，可以放在等号左边的值，现在被重新解释为location，可以理解为某已知的内存区域。
* **右值**：rvalue，放在等号右边的值。与左值相反，是不可知的区域　　　

在有些文章中，还有以下表述

* **左值** ：表达式结束后依然存在的**持久对象**。
* **右值** ：表达式结束后就不再存在的**临时对象**。

例如：

```cpp
int foo;
foo = 1;
```

上述代码中，`foo`是左值

```cpp
int foo(){
  return 1;
}
int main(){
  foo() = 2
}
```

上述代码是错误的，因为函数foo的返回值是临时变量，不可以作为左值，但是可以作为右值。

#### emplace\_back效率高

首先需要明确的是，`emplace_back`的效率比`push_back`高。`emplace_back`的最大优势是它可以直接在vector的内存中去构建对象，不用在外面构造完了再copy或者move进vector中。

### Reserve vs Resize

其实平时用的最多的是`resize`方法，其作用是改变`size=_Mylast - _Myfirst`

而reserve方法是用来改变`capacity=_Myend - _Myfirst`

### Reference

[STL vector的内部实现原理及基本用法](https://blog.csdn.net/u012658346/article/details/50725933)

[emplace\_back VS push\_back](https://haoqchen.site/2020/01/17/emplace\_back-vs-push\_back/)

#### set

#### map

#### swap

#### Reference

[https://github.com/arkingc/note/blob/master/C%2B%2B/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90.md](https://github.com/arkingc/note/blob/master/C%2B%2B/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90.md)

[https://github.com/steveLauwh/SGI-STL](https://github.com/steveLauwh/SGI-STL)

[https://github.com/karottc/sgi-stl](https://github.com/karottc/sgi-stl)



# C++

#### Basic

**Constant variable**

In C++, we use `#define` or `const` to define a constant variable

```c
#define ONE 1
const int ONE = 1;
```

All constant variable is stored in `Initialized Data Segment` (source of image: [https://www.geeksforgeeks.org/memory-layout-of-c-program/](https://www.geeksforgeeks.org/memory-layout-of-c-program/))

**Why should we use `const` as possible**

* Security: to avoid the problem of modifying code or data by mistake.
* usability: it would not affect the original function with `const`

**volatile**

volatile is the opposite of `const`. It shows that the variable is easy to change, so the compiler should not make any optimization to it. [https://cloud.tencent.com/developer/article/1637791](https://cloud.tencent.com/developer/article/1637791)

**Pointer const vs const pointer**

```c
char foo[] = "bar";
char* p = foo;                 // non-const pointer, non-const data
const char* p = foo;           // non-const pointer, const data
char* const p = foo;           // const pointer, non-const data
const char* const p = foo;     // const pointer, const data
```

In `const char* p = foo`; pointer p could point to other address, but we cannot modify \*p

**Variable**

Different from constant variable, variable is stored in stack or heap section.

**explicit keyword**

[https://www.cnblogs.com/this-543273659/archive/2011/08/02/2124596.html](https://www.cnblogs.com/this-543273659/archive/2011/08/02/2124596.html)

#### Type cast

[https://blog.csdn.net/shuzfan/article/details/77338366](https://blog.csdn.net/shuzfan/article/details/77338366)

**implicit type cast**

[https://www.cnblogs.com/apocelipes/p/14415033.html](https://www.cnblogs.com/apocelipes/p/14415033.html)

```c
int a = 0;
long long b = a + 1; // now variable a is long long
```

Rust can also support implicit type cast However, some programming languages **don't** support implicit type cast, such as golang

#### Explicit type cast

There are four explicit type cast in total:

1. static\_cast
2. dynamic\_cast
3. const\_cast
4. reinterpret\_cast

**Dangling Pointer and Wild Pointer**

* dangling pointer: the address has been released, but the pointer is not set to NULL

Example of dangling pointer:

```c
int* foo() {
    int a;     
    a = 100;
    return &a;
}                   
```

As shown above, when the function `foo` finishes, it returns the address of variable a. However, a is the local variable so it is stored in the stack. Therefore, the stack will be clear when the function returns, but \&a remained, leading to a Dangling pointer problem.

* wild pointer: the pointers which are not initialized.

**偏特化**

[https://harttle.land/2015/10/03/cpp-template.html](https://harttle.land/2015/10/03/cpp-template.html)

**变长模板参数**

**CRTP、RAII、Pimpl**

**sgi stl 内存池**

[https://fl0.top/2021/12/11/SGI](https://fl0.top/2021/12/11/SGI) STL内存配置器分析/

**vptr**

[https://cloud.tencent.com/developer/article/1510207](https://cloud.tencent.com/developer/article/1510207)

**generic programming 泛型編程**

函数模板和类模板

[https://blog.csdn.net/qq\_36086861/article/details/84785960](https://blog.csdn.net/qq\_36086861/article/details/84785960)

[https://juejin.cn/post/7056671589118509086](https://juejin.cn/post/7056671589118509086)

**Template Metaprogramming**

[https://harttle.land/2015/09/16/effective-cpp-48.html](https://harttle.land/2015/09/16/effective-cpp-48.html) [https://zhuanlan.zhihu.com/p/137853957](https://zhuanlan.zhihu.com/p/137853957)

[https://blog.csdn.net/AngelTempt/article/details/5541896](https://blog.csdn.net/AngelTempt/article/details/5541896)

将更多的任务放在编译时刻去完成 (like Rust)

**C++编译后的程序内存模型**

[https://blog.csdn.net/u014157109/article/details/115209061](https://blog.csdn.net/u014157109/article/details/115209061)

[https://zhuanlan.zhihu.com/p/184957568](https://zhuanlan.zhihu.com/p/184957568)

**What is the maximum value of stack in a C++ program?**

ubuntu默认是8mb， 可以用ulimit 来查看

```shell
ulimit -s
​
# in my Ubuntu, the output is 8192
```

C语言参数压栈顺序： 从右往左

**C++ features**

[https://www.jianshu.com/p/8c4952e9edec](https://www.jianshu.com/p/8c4952e9edec)

**C++ 11 features**

* auto
* decltype
* 列表初始化
* lambda表达式
* foreach语法糖
* 右值引用, [https://www.zhihu.com/question/22111546#:\~:text=](https://www.zhihu.com/question/22111546)右值引用的意义,本文主要讨论移动语义。\&text=移动语义，简单来说,诟病的问题之一。

```c
int && a = 10;
```

右值引用的好处：

[https://www.zhihu.com/question/22111546#:\~:text=%E5%8F%B3%E5%80%BC%E5%BC%95%E7%94%A8%E6%98%AFC,std%3A%3Afunction%EF%BC%89%E6%88%90%E4%B8%BA%E5%8F%AF%E8%83%BD%E3%80%82](https://www.zhihu.com/question/22111546)

* nullptr
* smart pointer
* std::move，std::forward，移动构造: ????? 移动语义

**C++ 14 特性**

* 智能指针中的make\_unique
* 函数返回类型推导
* ….

**C++ 17 特性**

* ….

**C++ 20 特性**

* 协程

**C++ RTTI**

**详细介绍实现过程：**

**RTTI (Run-time type information) 主要用于动态类型加载，** C++中实现RTTI主要是通过typeid运算符和dynamic\_cast运算符

**Typeid**

typeid来获取一个**变量**的类型

**dynamic\_cast**

以下代码会报错：

```c
#include <iostream>
using namespace std;
class B {};
class D : public B {};
int main()
{
    B* b = new D; // Base class pointer
    D* d = dynamic_cast<D*>(b); // Derived class pointer
    if (d != NULL)
        cout << "works";
    else
        cout << "cannot cast B* to D*";
    getchar(); // to get the next character
    return 0;
}
```

改进：增加虚函数

```c
#include <iostream>
using namespace std;
class B {
    virtual void fun() {}
};
class D : public B {
};
int main()
{
    B* b = new D; // Base class pointer
    D* d = dynamic_cast<D*>(b); // Derived class pointer
    if (d != NULL)
        cout << "works";
    else
        cout << "cannot cast B* to D*";
    getchar();
    return 0;
} 
​
// output:
// works
```

[https://blog.csdn.net/ljianhui/article/details/46487951](https://blog.csdn.net/ljianhui/article/details/46487951)

除了dynamic\_cast， 还有const\_cast， reinterpret\_cast等等

#### C++ OOP 特性:

继承，多态，封装

多态以及多态的实现方式

**多态**

多态的实现方式有3种：

* 虚函数：动态绑定，在运行时才能确定

```c
class A{
public:
    virtual void fun(){
        cout << "hello A" << endl;
    }
};

class B:public A{
public:
    virtual void fun(){
        cout << "hello B" << endl;
    }
};

int main(){
    A *a = new A();
    a->fun();
    a = new B();
    a->fun();
    return 0;
}
// out
/*
hello A
hello B
*/
```

* 重载：静态绑定，编译时刻可以确定

```c
int foo(int x){
    
}

int foo2(int x, int y){
    
}
```

* 模板：静态绑定，编译时刻可以确定

```c
template <class T>
T sum(T a, T b){
    return a + b;
}
```

**拷贝构造**

[https://www.runoob.com/cplusplus/cpp-copy-constructor.html](https://www.runoob.com/cplusplus/cpp-copy-constructor.html)

**static**

**虚函数的原理**

虚函数主要通过虚函数表（Vritual-Table）实现的， 每个包含了虚函数的类都包含一个虚函数表。

```c
#include <bits/stdc++.h>
using namespace std;

class Base{
public:
    void hello(){
        cout << "in Base\n" ;
    }
};

class Derive: public  Base{
public:
    void hello(){
        cout << "in Derive\n";
    }
};

int main(){
    Base* b = new Derive();
    b -> hello();
}
```

[https://segmentfault.com/a/1190000023597934#:\~:text=C%2B%2B%E4%B8%AD%E7%9A%84%E8%99%9A,%E6%9C%89%E2%80%9C%E5%A4%9A%E7%A7%8D%E5%BD%A2%E6%80%81%E2%80%9D%E3%80%82](https://segmentfault.com/a/1190000023597934)

构造函数一定不是虚函数

析构函数往往是虚函数： 一般默认构造函数不是虚函数，其他的是虚函数

**C++ 类内可以定义引用数据成员**

[https://blog.csdn.net/ArmyShen/article/details/8684473](https://blog.csdn.net/ArmyShen/article/details/8684473)

#### C++ 设计模式

**C++实现严格的单例模式**

**智能指针**

C++11引入了三个新的智能指针：

* unique\_ptr
* shared\_ptr
* weak\_ptr

11版本之前的auto\_ptr由于设计缺陷问题，被unique\_ptr代替了（具体是什么缺陷以后再讲）

被废弃的原因： [https://zhuanlan.zhihu.com/p/356627164](https://zhuanlan.zhihu.com/p/356627164)

[https://zhuanlan.zhihu.com/p/533807432](https://zhuanlan.zhihu.com/p/533807432)

[https://blog.csdn.net/K346K346/article/details/81478223](https://blog.csdn.net/K346K346/article/details/81478223)

[http://c.biancheng.net/view/7898.html](http://c.biancheng.net/view/7898.html)

[https://docs.microsoft.com/zh-cn/cpp/cpp/smart-pointers-modern-cpp?view=msvc-170](https://docs.microsoft.com/zh-cn/cpp/cpp/smart-pointers-modern-cpp?view=msvc-170)

```c
#include<iostream>
#include<memory>
using namespace  std;
struct SomeData{
    int a, b, c;
};
c
void f(){
//    without smart pointer
//    SomeData* data = new SomeData;

//    with smart pointer
    auto data = make_unique<SomeData>();
    data -> a = 1;
    data -> b = 2;
    data -> c = 3;
}
```

#### C++ STL

**vector**

至今还没找到vector是在哪个版本引入的

[https://zhuanlan.zhihu.com/p/377186496](https://zhuanlan.zhihu.com/p/377186496)

**Set and unordered\_set**

底层是红黑树

**Map and unordered\_map**

1. Map： 底层是红黑树，具有排序功能
2. unordered\_map： C++11版本新加的，用来代替hash\_map，大多数场景下，效率比map高

[https://blog.csdn.net/weixin\_43202635/article/details/115053830](https://blog.csdn.net/weixin\_43202635/article/details/115053830)

[https://blog.csdn.net/yangxuan0261/article/details/52090128](https://blog.csdn.net/yangxuan0261/article/details/52090128)

**list**

双向循环链表

**Swap 源码分析**

[https://blog.csdn.net/weixin\_38739799/article/details/80743030](https://blog.csdn.net/weixin\_38739799/article/details/80743030)

[https://www.jianshu.com/p/72e1a386e449](https://www.jianshu.com/p/72e1a386e449)

**Reverse 源码分析**

**stl中不同容器的底层实现，和各自的迭代器失效（list vector map unordered\_map deque等**

**Sort 源码分析**

[https://feihu.me/blog/2014/sgi-std-sort/](https://feihu.me/blog/2014/sgi-std-sort/)

#### C++并发编程

**锁**

**C++ 线程池**

[https://cloud.tencent.com/developer/article/1711459](https://cloud.tencent.com/developer/article/1711459)

**协程 Coroutine**

[https://zhuanlan.zhihu.com/p/59178345](https://zhuanlan.zhihu.com/p/59178345)

python 协程： [https://www.liaoxuefeng.com/wiki/1016959663602400/1017968846697824](https://www.liaoxuefeng.com/wiki/1016959663602400/1017968846697824)

Go 协程：

C++ 多线程 示例代码

```c
#include <iostream>
#include <vector>
#include <algorithm>
#include <thread>
#include <numeric>
#include <time.h>
using namespace std;

//线程要做的事情就写在这个线程函数中
void GetSumT(vector<int>::iterator first,vector<int>::iterator last,int &result)
{
    result = accumulate(first,last,0); //调用C++标准库算法
}

int main() //主线程
{
    long startTime = time(nullptr);
    int result1,result2,result3,result4,result5;
    vector<int> largeArrays;
    for(int i=0;i<100000000;i++)
    {
        if(i%2==0)
            largeArrays.push_back(i);
        else
            largeArrays.push_back(-1*i);
    }
    thread first(GetSumT,largeArrays.begin(),
                 largeArrays.begin()+20000000,std::ref(result1)); //子线程1
    thread second(GetSumT,largeArrays.begin()+20000000,
                  largeArrays.begin()+40000000,std::ref(result2)); //子线程2
    thread third(GetSumT,largeArrays.begin()+40000000,
                 largeArrays.begin()+60000000,std::ref(result3)); //子线程3
    thread fouth(GetSumT,largeArrays.begin()+60000000,
                 largeArrays.begin()+80000000,std::ref(result4)); //子线程4
    thread fifth(GetSumT,largeArrays.begin()+80000000,
                 largeArrays.end(),std::ref(result5)); //子线程5

    first.join(); //主线程要等待子线程执行完毕
    second.join();
    third.join();
    fouth.join();
    fifth.join();

    int resultSum = result1+result2+result3+result4+result5; //汇总各个子线程的结果
    cout << resultSum << endl;
    long endTime = time(nullptr);
    cout << endTime - startTime << endl;

    return 0;
}
```

#### C++和C的区别

new/delete与malloc/free的区别是什么

[https://cloud.tencent.com/developer/article/1792158](https://cloud.tencent.com/developer/article/1792158)

[https://blog.csdn.net/u010510020/article/details/76266505](https://blog.csdn.net/u010510020/article/details/76266505)

extern “C” ===> C++调用C函数需要extern C，因为C语言没有函数重载。

**NULL 和 nullptr**

C++相比于C，可以使用nullptr表示空。

[https://blog.csdn.net/qq\_38410730/article/details/105183769](https://blog.csdn.net/qq\_38410730/article/details/105183769)

**c的malloc和c++ new的区别**

[https://blog.51cto.com/u\_14289397/2540190#:\~:text=1.new%E6%98%AFC%2B%2B,%E6%89%80%E9%9C%80%E5%86%85%E5%AD%98%E7%9A%84%E5%B0%BA%E5%AF%B8%E3%80%82](https://blog.51cto.com/u\_14289397/2540190)

* 第一级分配器
    * 直接使用malloc()和free()
* 第二级分配器
    * 当分配区块超过128bytes时，视为“足够大”，调用第一级分配器
    * 当分配区块小于128bytes时，视为“过小”，为了降低额外负担，采用复杂的memory pool整理方式，不再求助于第一级分配器

#### C++ 其他

**Ｃ++ 指针的一个坑**

函数中传递的指针是传值，也就是传递一个拷贝，而不是传引用

```c
void func1(linkedList* head){
	head = head -> next;
}

void func2(linkedList* head){
    head -> next = nullptr;
}

int main(){
	func1(head);  // head will not change
    func2(head);  // head will point to NULL
}
```

```c
#include <iostream>
using namespace std;

struct node
{
    int val;
    node(int _val): val(_val){}
};

void func1(node *head)
{
    head -> val = 2;
    return;
}

void func2(node *head) {
    node* t = new node(3);
    head = t;
    return;
}

void func3(node head){
    head.val = 5;
    return;
}

int main()
{
    node* head = new node(1);
    func1(head);
    cout << head -> val << endl;
    func2(head);
    cout << head -> val << endl;
    node n1 = {1};
    func3(n1);
    cout << n1.val << endl;
}

/*
output:
2
2
1
*/
```

这个例子比较直观，核心点是，在函数中，形参和实参共同指向了同一个地址，所以在func1中，修改val的值的时候，地址上的内容也被修改了。但是在func2中，形参head指向了t，原来的实参没有改变。在func3中，传入的是结构体的值，所以形参只是个拷贝，不会对实参产生变化。

**未知行数的输入**

```c
#include <bits/stdc++.h>
using namespace std;

int main() {
    string s;
    while (getline(cin, s)) {
        if(s.length() == 0){
            break;
        }
        int i = stoi(s);
        cout << i << endl;
    }
}
```

**堆栈**

[https://blog.csdn.net/summer00072/article/details/80861818#:\~:text=%E5%9C%A8Windows%E4%B8%8B%EF%BC%8C%E6%A0%88%E6%98%AF,%E9%80%9A%E8%BF%87ulimit%20%2Ds%E6%9D%A5%E8%AE%BE%E7%BD%AE%E3%80%82](https://blog.csdn.net/summer00072/article/details/80861818)

[**https://github.com/huihut/interview#link-loading-library**](https://github.com/huihut/interview#link-loading-library)
