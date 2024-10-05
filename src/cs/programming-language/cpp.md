# Revisiting C++ Foundations

## Chapter 1: Variables and Basic Types
Type conversion：

```cpp
bool b = 42; // b is true
int i = b; // i has value 1
i = 3.14; // i has value 3
double pi = i; // pi has value 3.0
unsigned char c = -1; // assuming 8-bit chars, c has value 255
signed char c2 = 256; // assuming 8-bit chars, the value of c2 is undefined
```

#### Variables

**Initialization**

List Initialization:

```cpp
int units_sold = 0;
int units_sold = {0};
int units_sold{0};
int units_sold(0);
long double ld = 3.1415926536;
int a{ld}, b = {ld}; // error: narrowing conversion required
int c(ld), d = ld; // ok: but value will be truncated
```

**Variable Declarations and Definitions**

```cpp
extern int i; // declares but does not define i
int j; // declares and defines j
extern double pi = 3.1416; // definition
```

**Identifiers**

* letters
* digits
* underscore character

Some rules:

* An identifier should give some indication of its meaning.
* Variable names normally are lowercase—index, not Index or INDEX.
* Like Sales\_item, classes we define usually begin with an uppercase letter.
* Identifiers with multiple words should visually distinguish each word, for example, student\_loan or studentLoan, not studentloan.

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/img/20221114231610.png)

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/img/20221114231631.png)

#### Compound Types

NOTE: Technically speaking, when we use the term reference, we mean “lvalue reference.”

```cpp
int ival = 1024;
int &refVal = ival; // refVal refers to (is another name for) ival
int &refVal2; // error: a reference must be initialized
```

Understanding Compound Type Declarations:

```cpp
// i is an int; p is a pointer to int; r is a reference to int
int i = 1024, *p = &i, &r = i;

int* p; // legal but might be misleading
int* p1, p2; // p1 is a pointer to int; p2 is an int
int *p1, *p2; // both p1 and p2 are pointers to int

int ival = 1024;
int *pi = &ival; // pi points to an int
int **ppi = &pi; // ppi points to a pointer to an int
```

#### const Qualifier

```cpp
int i = 42;
const int ci = i; // ok: the value in i is copied into ci
int j = ci; // ok: the value in ci is copied into j
```

By Default, const Objects Are Local to a File. To define a single instance of a const variable, we use the keyword extern on both its definition and declaration(s):

```cpp
// file_1.cc defines and initializes a const that is accessible to other files
extern const int bufSize = fcn();
// file_1.h
extern const int bufSize; // same bufSize as defined in file_1.cc

```

```cpp
const int ci = 1024;
const int &r1 = ci; // ok: both reference and underlying object are const
r1 = 42; // error: r1 is a reference to const
int &r2 = ci; // error: nonconst reference to a const object
```

#### Type Aliases

```cpp
typedef double wages; // wages is a synonym for double
typedef wages base, *p; // base is a synonym for double, p for double*
```

**auto and decltype**

```cpp
auto i = 0, *p = &i; // ok: i is int and p is a pointer to int
auto sz = 0, pi = 3.14; // error: inconsistent types for sz and pi
```

```cpp
decltype(f()) sum = x; // sum has whatever type f return

const int ci = 0, &cj = ci;
decltype(ci) x = 0; // x has type const int
decltype(cj) y = x; // y has type const int& and is bound to x
decltype(cj) z; // error: z is a reference and must be initialized
```

## Chapter 2 Strings, Vectors, and Arrays

#### string

Ways to Initialize a string:

```cpp
string s1 //Default initialization; s1 is the empty string.
string s2(s1) //s2 is a copy of s1.
string s2 = s1 //Equivalent to s2(s1), s2 is a copy of s1.
string s3("value") //s3 is a copy of the string literal, not including the null.
string s3 = "value" //Equivalent to s3("value"), s3 is a copy of the string literal.
string s4(n, 'c') //Initialize s4 with n copies of the character 'c'.
```

string Operations:

```cpp
os << s //Writes s onto output stream os. Returns os.
is >> s //Reads whitespace-separated string from is into s. Returns is.
getline(is, s) //Reads a line of input from is into s. Returns is.
s.empty() //Returns true if s is empty; otherwise returns false.
s.size() //Returns the number of characters in s.
s[n] //Returns a reference to the char at position n in s; positions start at 0.
s1 + s2 //Returns a string that is the concatenation of s1 and s2.
s1 = s2 //Replaces characters in s1 with a copy of s2.
s1 == s2
s1 != s2
// The strings s1 and s2 are equal if they contain the same characters.
// Equality is case-sensitive.
<, <=, >, >=  //Comparisons are case-sensitive and use dictionary ordering
```

cctype Functions:

```cpp
isalnum(c)// true if c is a letter or a digit.
isalpha(c) true if //c is a letter.
iscntrl(c) true if //c is a control character.
isdigit(c) true if //c is a digit.
isgraph(c) true if //c is not a space but is printable.
islower(c) true if //c is a lowercase letter.
isprint(c) true if //c is a printable character (i.e., a space or a character that has a
visible representation).
ispunct(c) true if //c is a punctuation character (i.e., a character that is not a control
character, a digit, a letter, or a printable whitespace).
isspace(c) true if //c is whitespace (i.e., a space, tab, vertical tab, return, newline, or
formfeed).
isupper(c) true if //c is an uppercase letter.
isxdigit(c) true if //c is a hexadecimal digit.
tolower(c) //If c is an uppercase letter, returns its lowercase equivalent; otherwise
returns c unchanged.
toupper(c) //If c is a lowercase letter, returns its uppercase equivalent; otherwise returns
c unchanged
```

#### vector

```cpp
vector<int> v1(10); // v1 has ten elements with value 0
vector<int> v2{10}; // v2 has one element with value 10
vector<int> v3(10, 1); // v3 has ten elements with value 1
vector<int> v4{10, 1}; // v4 has two elements with values 10 and 1
```

## Chapter 3 Expressions

**Implicit Conversions**

bool flag; char cval; short sval; unsigned short usval; int ival; unsigned int uival; long lval; unsigned long ulval; float fval; double dval;

```cpp
3.14159L + 'a'; // 'a' promoted to int, then that int converted to long double
dval + ival; // ival converted to double
dval + fval; // fval converted to double
ival = dval; // dval converted (by truncation) to int
flag = dval; // if dval is 0, then flag is false, otherwise true
cval + fval; // cval promoted to int, then that int converted to float
sval + cval; // sval and cval promoted to int
cval + lval; // cval converted to long
ival + ulval; // ival converted to unsigned long
usval + ival; // promotion depends on the size of unsigned short and int
uival + lval; // conversion depends on the size of unsigned int and long


int ia[10]; // array of ten ints
int* ip = ia; // convert ia to a pointer to the first element

char *cp = get_string();
if (cp) /* ... */ // true if the pointer cp is not zero
while (*cp) /* ... */ // true if *cp is not the null character

int i;
const int &j = i; // convert a nonconst to a reference to const int
const int *p = &i; // convert address of a nonconst to the address of a const
int &r = j, *q = p; // error: conversion from const to nonconst not allowed

string s, t = "a value"; // character string literal converted to type string
while (cin >> s) // while condition converts cin to bool
```

**Explicit Conversion**

```cpp
cast-name<type>(expression);
```

**static\_cast**

A static\_cast is often useful when a larger arithmetic type is assigned to a smaller type.

```cpp
// cast used to force floating-point division
double slope = static_cast<double>(j) / i;

void* p = &d; // ok: address of any nonconst object can be stored in a void*
// ok: converts void* back to the original pointer type
double *dp = static_cast<double*>(p);
```

**const\_cast**

Conventionally we say that a cast that converts a const object to a non-const type “casts away the const.”

```cpp
const char *pc;
char *p = const_cast<char*>(pc); // ok: but writing through p is undefined
```

Only a const\_cast may be used to change the constness of an expression.

```cpp
const char *cp;
// error: static_cast can’t cast away const
char *q = static_cast<char*>(cp);
static_cast<string>(cp); // ok: converts string literal to string
const_cast<string>(cp); // error: const_cast only changes constness
```

**reinterpret\_cast**

A reinterpret\_cast generally performs a low-level reinterpretation of the bit pattern of its operands.

```cpp
int *ip;
char *pc = reinterpret_cast<char*>(ip);
```

**dynamic\_cast**

dynamic\_cast is used at run time to find out correct down-cast

A dynamic\_cast has the following form:

```cpp
dynamic_cast<type*>(e)
dynamic_cast<type&>(e)
dynamic_cast<type&&>(e)
```

dynamic\_cast will be covered in RTTI

**Old-Style Casts**

```cpp
type (expr); // function-style cast notation
(type) expr; // C-language-style cast notation
```

**Operator precedence table**

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/img/20221115165506.png)

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/img/20221115165523.png)

## Chapter 4: Algorithms

## Sorting
```cpp
vector<int> arr;
sort(arr.begin(), arr.end()); 
sort(arr.rbegin(), arr.rend()); // reverse sorting

int a[n];
sort(a, a + n); // ascending default
// only sort part of array
sort(a, a + p); // where p < n
sort(a, a + n, greater<int>()) // for descending

vector<vector<int>> mat
auto cmp = [&](const vector<int>& arr1, const vector<int>& arr2){
    return arr1[1] < arr2[1];
};
sort(mat.begin(), mat.end(), cmp);

// priority queue
auto cmp = [&](pair<int, int>& p1, pair<int, int>&p2){
    return p1.second < p2.second;
};

priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> pq(cmp);
```

#### Select Sorting

```cpp
void selectSort(vector<int>& arr){
    for(int i = 0; i < arr.size(); i++){
        int minIndex = i;
        for(int j = i + 1; j < arr.size(); j++){
            if(arr[j] < arr[minIndex]){
                minIndex = j;
            }
        }
        if(minIndex != i){
            swap(arr[i], arr[minIndex]);
        }
    }
}
```

#### Quick Sorting
```cpp
void Qsort(int left, int right, vector<int>&nums){
        if(left >= right){
            return;
        }
        int i = left;
        for(int j = left; j < right; ++j){
            if(nums[j] <= nums[right]){
                swap(nums[i++], nums[j]);
            }
        }
        swap(nums[i], nums[right]);
        Qsort(left,  i - 1, nums);
        Qsort(i + 1, right, nums);
    }
```

#### Heap Sorting
```cpp
void heapify(vector<int>&nums, int n, int p){
        int old = p;
        int l = p * 2 + 1;
        int r = p * 2 + 2;
        if(l < n && nums[p] < nums[l]){
            p = l;
        }
        if(r < n && nums[p] < nums[r]){
            p = r;
        }
        if(p != old){
            swap(nums[p], nums[old]);
            heapify(nums, n, p);
        }
        
    }
    
   void heapSort(vector<int>& nums){
       for(int i = nums.size() / 2 - 1; i >= 0; i--){
           heapify(nums, nums.size(), i);
       }
       for(int i = nums.size() - 1; i >= 0; i--){
           swap(nums[0], nums[i]);
           heapify(nums, i, 0);
       }
   }
```

#### Bubble Sorting
```cpp
void bubbleSort(vector<int>& arr) {
    for(int i = arr.size() - 1 ; i >= 0; i--){
        for(int j = 1; j <= i; j++){
            if(arr[j - 1] > arr[j]){
                swap(arr[j], arr[j-1]);
            }
        }
    }
}
```

#### Insert Sorting
```cpp
void insertSort(vector<int>& arr) {
    for(int i = 0; i < arr.size() - 1; i++){
        for(int j = i + 1; j > 0; j--){
            if(arr[j-1] > arr[j])
     	        swap(arr[j-1], arr[j]);
            else
                break;
        }
    }
}
```

#### Merge Sorting
```cpp
void merge(vector<int> &nums, int left, int right){
    if (left >= right) {
        return;
    }
    int mid = left + right >> 1;
    merge(nums, left, mid);
    merge(nums, mid + 1, right);
    vector<int> tmp(right - left + 1);
    int idx = 0;
    int i = left, j = mid + 1;
    while (i <= mid and j <= right) {
        if (nums[i] <= nums[j]) {
            tmp[idx++] = nums[i++];
        } else {
            tmp[idx++] = nums[j++];
        };
    }
    while(i <= mid){
        tmp[idx++] = nums[i++];
    }
    while(j <= right) {
        tmp[idx++] = nums[j++];
    }
    i = left;
    for (auto x: tmp) {
        nums[i++] = x;
    }
}
void mergeSort(vector<int>& nums){
    merge(nums, 0, nums.size() - 1);
}
```

## Searching

### Binary Searching

```cpp
int left = START, right = END;
while (left <= right) {
    int mid = left + right >> 1;
    if (/*match*/) {
        right = mid - 1
    } else {
        left = mid + 1;
    }
}
```

Binary Search function in C++

```cpp
int l = lower_bound(arr.begin(), arr.end(), target) - arr.begin();
int u = upper_bound(arr.begin(), arr.begin(), target) - arr.begin();
```

## DP

### Knapsack problem

#### 0/1 Knapsack

```cpp
for (int i = 0; i < n; ++i){
    for (int j = amount; j >= weight[i]; --j) {
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

#### Complete Knapsack

```cpp
for (int i = 0; i < n; ++i){
    for (int j = weight[i]; j <= amount; ++j) {
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

#### bitmask DP

```cpp
int f[1 << 20][20];
int hamilton(int n, int weight[20][20]) {++
    memset(f, 0x3f, sizeof f);
    f[1][0] = 0;
    for(int i = 1; i < 1 << n; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i >> j & 1) {
                for (int k = 0; k < n; ++k) {
                    if(i - (1 << j) >> k & 1) {
                        f[i][j] = min(f[i][j], f[i - (1 << j)][k] + weight[j][k]);
                    }
                }
            }
        }
    }
    return f[(1<<n)- 1][n - 1];
}
```

## Chapter 5: Data Structures
### Union and Find
```cpp
int find(int x) {
  while (x != link[x]){
        x = link[x];
    }
    return x;
}

void union(int a, int b) {
    a = find(a);
    b = find(b);
    link[a] = b;
}

void union_find(){
    vector<int> link(n);
    for (int i = 0; i < n; ++i) {
        link[i] = i;
    }
}
```

### Dijkstra's Algorithm
```cpp
vector<int> dis(n, INT_MAX);
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
pq.emplace(0, startPos);
dis[startPos] = 0;
while (!pq.empty()) {
    auto cur = pq.top();
    pq.pop();
    int curPos = cur.second;
    int curDis = cur.first;
    for (int nextPos: adj[curPos]) {
        if (dis[nextPos] > curDis + edge[curPos][nextPos]) {
            dis[nextPos] = curDis + edge[curPos][nextPos];
            pq.emplace(dis[nextPos], nextPos);
        }
    }
}
```

### suffix array
```cpp
#define ll long long

vector<int> suffixArray(){
    string s;
    cin >> s;
    s += '$';
    int n = (int)s.size();
    vector<int> p(n), c(n);
    {
        vector<pair<char, int>> a(n);
        for (int i = 0; i < n; ++i) {
            a[i] = {s[i], i};
        }
        sort(a.begin(), a.end());
        for (int i = 0; i < n; ++i) {
            p[i] = a[i].second;
        }
        c[p[0]] = 0;
        for (int i = 1; i < n; ++i) {
            if (a[i].first == a[i-1].first) {
                c[p[i]] = c[p[i - 1]];
            } else {
                c[p[i]] = c[p[i - 1]] + 1;
            }
        }
    }
    int k = 0;
    while ((1 << k) < n) {
        vector<pair<pair<int, int>, int>>a(n);
        // update a according to c
        for (int i = 0; i < n; ++i) {
            a[i].first = {c[i], c[(i + (1 << k)) % n]};
            a[i].second = i;
        }
        sort(a.begin(), a.end());
        for (int i = 0; i < n; ++i) {
            p[i] = a[i].second;
        }
        c[p[0]] = 0;
        for (int i = 1; i < n; ++i) {
            if (a[i].first == a[i-1].first) {
                c[p[i]] = c[p[i - 1]];
            } else {
                c[p[i]] = c[p[i - 1]] + 1;
            }
        }
        k++;
    }
    return p;
}
```


### Segment Tree
```cpp
#define ll long long
struct segtree {
    int size;
    vector<ll> sums;
    void init(int n) {
        size = 1;
        while (size < n) {
            size *= 2;
        }
        sums.assign(2 * size, 0LL);
    }

    void set(int i, int v, int x, int lx, int rx) {
        if (rx - lx == 1) {
            sums[x] = v;
            return;
        }
        int m = (lx + rx) / 2;
        if (i < m) {
            set(i, v, 2 * x + 1, lx, m);
        } else {
            set(i, v, 2 * x + 2, m, rx);
        }
        sums[x] = sums[2 * x + 1] + sums[2 * x + 2];
    }
    void set(int i, int v) {
        set(i, v, 0, 0, size);
    }
    ll sum(int l, int r, int x, int lx, int rx) {
        if(lx >= r or rx <= l) {
            return 0;
        }
        if(lx >= l and rx <= r) {
            return sums[x];
        }
        int m = (lx + rx) / 2;
        ll s1 = sum(l, r, 2 * x + 1, lx, m);
        ll s2 = sum(l, r, 2 * x + 2, m, rx);
        return s1 + s2;
    }
    ll sum(int l, int r){
        return sum(l, r, 0, 0, size);
    }
};
```

## Appendix A
### bits-stdc++
[bits-stdc++.h](https://gist.githubusercontent.com/ryqdev/a9a9d745ece71d50f84630319c9abcca/raw/c2d66814feab32b7d4450c0810cc1622523c0efe/bits-stdc++.h)

### debug.h
[debug.h](https://gist.githubusercontent.com/ryqdev/932e063be3bfaeec1809c276554a4209/raw/90c8fca18be02682cbadd3a1c55a038dfa4173e4/debug.h)

## References
- Stanley B. Lippman. C++ Primer Notes