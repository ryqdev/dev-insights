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