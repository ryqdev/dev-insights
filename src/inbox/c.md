# C Programming Language

```c
#include <stdio.h>
#include <unistd.h>
int main() {
    usleep(5000000); // microsecond = 10^-6 s
    printf("hello\n");
    return 0;
}
```

sizeof vs strlen
```c
#include <stdio.h>
#include <string.h>
​
int main() {
    char str[80] = "abcde";
    printf("sizeof(str) = %lu\n", sizeof(str));     //80
​
    char str1[] = "abcde";
    printf("sizeof(str1) = %lu\n", sizeof(str1));   //6
​
    char str2[] = "\0abcde";
    printf("sizeof(str2) = %lu\n", sizeof(str2));   //7
​
    char str3[] = "abcde";
    printf("strlen(str3) = %lu\n", strlen(str3));   //5
​
    char str4[100] = "abcde";
    printf("strlen(str4) = %lu\n", strlen(str4));   //5
​
    char str5[] = "\0abcde";
    printf("strlen(str5) = %lu\n", strlen(str5));   //0
​
    int a[] = {1,2,3};
    printf("sizeof(a) = %lu\n", sizeof(a));     //12 = 3 * 4 bytes
​
    int b[] = {1,2,3};
    printf("strlen(b) = %lu\n", strlen(b));   //1; strlen should only be used in string
​
    return 0;
}
```

sprintf、strcpy和memcpy的区别 这三个函数都是用来复制的，但是需要辨析其中的区别

* sprintf 可以用%s来实现格式化写入，其他两个做不到。
* strcpy 遇到\0结束（\0也被复制了），只能拷贝字符串。对于拷贝字符串，选择strcpy最佳
* memcpy 根据size大小来复制，可以复制各种数据类型（结构体、数组）。

源码实现：

```c
char *strcpy(char *strDest, const char *strSrc);
{
　　if ((strSrc == NULL) || (strDest == NULL))
　　{
　　　　　return NULL;
　　}
　　char *dest = strDest;
　　while ((*strDest++ = *strSrc++)!='\0');
　　return dest;
}
```

```c
void *memcpy(void *memTo, const void *memFrom, size_t size)
{
　　if((memTo == NULL) || (memFrom == NULL))
         return NULL;
　　char *tempFrom = (char *)memFrom;
　　char *tempTo = (char *)memTo;
　　while(size -- > 0)
       　　*tempTo++ = *tempFrom++ ; 
　　return memTo;
}
```

#### 数组指针与指针数组区别

* 数组指针

```c
int a[3][4];
int (*p)[4];// 定义一个数组指针，指向含有四个元素的一维数组
p=a;// 将二维数组a的首地址赋值给p，也就是a[0]或&a[0][0]
p++;// 执行过后，也就是p=p+1，p跨过行a[0]指向行a[1]，也就是跨过4个步长
// 所以数组指针也称为一维数组的指针，也叫行指针。
```

* 指针数组

```c
int *p[3];// 指针数组，可以理解成*p本身就是个数组
int a[3][4];
for(i=0;i<3;i++)
    p[i] = a[i];
```

#### 数组和指针的区别

**sizeof的结果不一样**

```c
#include <stdio.h>
#include <string.h>
​
int main() {
    int a[] = {1,2,3};
    printf("sizeof(a) = %lu\n", sizeof(a)); // sizeof(a) = 12
    int *p = a;
    printf("sizeof(p) = %lu\n", sizeof(p)); // sizeof(p) = 8
    // 在32位平台下，无论指针的类型是什么，sizeof（指针名）都是4，在64位平台下，无论指针的类型是什么，sizeof（指针名）都是8。
}
```

#### program memory layout of C/C++

* Text segment: executable code of the program
* Data segment: static/global variables that are initialized (const variable might be stored here)
* BSS: uninitialized static/global variables
* Heap: space for dynamic memory
* Stack: local variables, return address, arguments …

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/imgScreenshot%20from%202022-11-21%2016-14-21\_skljfdsf.png)

#### Decimal to Hexadecimal Converter1

#### Magic Number

Fast Inverse Square Root

```c
float InvSqrt(float x)
{
    float xhalf = 0.5f * x;
    int i = *(int *)&x;
    i = 0x5f3759df - (i>>1);
    x = *(float *)&i;
    x = x * (1.5f - xhalf * x * x);
    return 1/x;
}
```

### Reference:

[https://blog.csdn.net/lz0499/article/details/78238597](https://blog.csdn.net/lz0499/article/details/78238597)

#### Measure time in C

```c
#include <stdio.h>
#include <time.h>
​
void fun()
{
  int n = 1e8;
  for (int i = 0; i < n; ++i) {
    ;
  }
}
​
int main()
{
  clock_t start, end;
  start = clock();
  fun();
  end = clock();
  double time_taken = ((double)end - start)/CLOCKS_PER_SEC; // in seconds
​
  printf("It takes %f seconds to execute \n", time_taken);
  return 0;
}
​
```
