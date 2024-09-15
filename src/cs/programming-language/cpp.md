# Revisiting C++ Foundations

## Chapter 1: Introduction to C++

## Chapter 2: Variables and Basic Types

## Chapter 3: Data Structures
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
