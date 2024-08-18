# Data Structure

## Segment Tree
The most simple version:
```cpp
struct SegTree{
    int size;
    vector<long long> sums;
 
    explicit SegTree(int n) {
        size = 1;
        while(size < n) size *= 2;
        sums.assign(size * 2, 0LL);
    }
 
    void set(int i, int v, int x, int xl, int xr) {
        if (xr - xl == 1) {
            sums[x] = v;
            return;
        }
        int m = xl + xr >> 1;
        if (i < m) {
            set(i, v, 2 * x + 1, xl, m);
        } else {
            set(i, v, 2 * x + 2, m, xr);
        }
        sums[x] = sums[2 * x + 1] + sums[2 * x + 2];
    }
 
    void set(int i, int v){
        set(i, v, 0, 0, size);
    }
 
    long long sum(int l, int r, int x, int xl, int xr) {
        if (l >= xr || r <= xl) return 0;
        if (l <= xl && r >= xr) return sums[x];
        return sum(l, r, 2 * x + 1, xl, xl + xr >> 1) + sum(l, r, 2 * x + 2, xl + xr >> 1, xr);
    }
 
    long long sum(int l, int r){
        return sum(l, r, 0, 0, size);
    }
};
```
