# Algorithm Tips

## Sorting
```cpp
vector<int> arr;
sort(arr.begin(), arr.end()); 
sort(arr.rbegin(), arr.rend()); // reverse sorting

int a[n];
sort(a, a + n); // ascending default
// only sort part of array
sort(a, a + p); // where p < n
sort(a, a + n,  greater<int>()) // for descending

vector<vector<int>> mat
auto cmp = [&](const vector<int>& arr1, const vector<int>& arr2){
        return arr1[1] < arr2[1];
};
sort(mat.begin(), mat.end(), cmp);

// priority queue
auto cmp = [&](pair<int, int>& p1, pair<int, int>&p2){
        return p1.second < p2.second;C++
};

priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> pq(cmp);
```

### Implementating Sorting in C++

TODO

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

## Hashing

C++里面常用的hashtable是unordered\_map，是C++11推出的全新的数据结构，用来代替map

**Hash碰撞了怎么办**

* 开链法
* 扩容 + rehash

**C++ 中使用hashMap**

在C++11之后，一般用`unordered_map`去使用hashMap，其底层是一个哈希表，当哈希碰撞的时候使用开链法，使用单向链表存储。当链表过大的时候，会将链表转化为红黑树

## Searching

### Binary Searching

二分查找看起来简单，但是在实际场景中很容易出现边界错误，以下是我常用的一个二分模板：

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

**C++ 二分法函数**

```cpp
int l = lower_bound(arr.begin(), arr.end(), target) - arr.begin();
int u = upper_bound(arr.begin(), arr.begin(), target) - arr.begin();
```

### **图搜索**

假设图中节点数量为v，边的数量为e

**深度优先遍历 DFS**

**时间复杂度：**

* 用邻接表存储的时候：O(v+e)
* 用邻接矩阵存储的时候: O(v^2)

**空间复杂度**

O(v)

**广度优先遍历 BFS**

**时间复杂度：**

* 用邻接表存储的时候：O (v+e)
* 用邻接矩阵存储的时候: O(v^2)

**空间复杂度：**

最坏O(v)

## DP

动态规划最核心的是描述状态，并且找到状态转移的表达式

背包问题是最经典动态规划问题, 下文将重点讲述背包问题

**背包问题**

例题

* [https://www.acwing.com/problem/content/2/](https://www.acwing.com/problem/content/2/)
* [https://leetcode.com/problems/coin-change/](https://leetcode.com/problems/coin-change/)

**0-1背包**

```cpp
for (int i = 0; i < n; ++i){
    for (int j = amount; j >= weight[i]; --j) {
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

**完全背包**

```cpp
for (int i = 0; i < n; ++i){
    for (int j = weight[i]; j <= amount; ++j) {
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
}
```

**多重背包**

面对多重背包问题，思路是将其转化为0-1背包问题

**状态压缩DP**

状态压缩是DP中降低复杂度的有效方法，以下以hamilton algorithm为例：

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

#### 连续区间问题

滑动窗口法

例题： [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) The problem is easy

But how about this:

[https://leetcode.com/problems/count-subarrays-with-fixed-bounds/solutions/](https://leetcode.com/problems/count-subarrays-with-fixed-bounds/solutions/)

#### bit operation

remove the rightmost set bit

```cpp
num = num & (num - 1);
```

get the number of set bit in binary number:

```cpp
int getSetBitCount(int num) {
    int cnt = 0;
    while (num) {
        num = num & (num - 1);
        cnt++;
    }
    return cnt;
}
```

lowbit operation:

```cpp
lowbit = n & (~n + 1);
lowbit = n & (-n);
```

#### Math problem

[https://leetcode.com/problems/minimum-addition-to-make-integer-beautiful/](https://leetcode.com/problems/minimum-addition-to-make-integer-beautiful/)

**bit operation**

remove the rightmost set bit

```cpp
num = num & (num - 1);
```

Get the number of set bits in binary number: low-bit operation:

```cpp
lowbit = n & (~n + 1);
lowbit = n & (-n);
```

change symbol:

```cpp
int reversal(int a) {
  return ~a + 1;
}
```

get abs value

```cpp
int abs2(int a) {
  int i = a >> 31;
  return ((a^i) - i);
}
```

example: [https://codeforces.com/contest/1632/problem/C](https://codeforces.com/contest/1632/problem/C)
