# Weekly Contest 401 

[3170. Lexicographically Minimum String After Removing Stars](https://leetcode.com/problems/lexicographically-minimum-string-after-removing-stars/description/)
```cpp
class Solution {
public:
    string clearStars(string s) {
        // build a heap with (char, pos)
        auto cmp = [](pair<char, int> a, pair<char, int> b)
        {
            if (a.first == b.first)
            {
                return a.second < b.second;
            }
            return a.first > b.first;
        };
        priority_queue<pair<char, int>, vector<pair<char, int>>, decltype(cmp)> pq;
        unordered_map<int, bool> e;
        int n = s.size();
        for (int i = 0; i < n; ++i)
        {
            if (s[i] == '*')
            {
                int pos = pq.top().second;
                pq.pop();
                e[pos] = true;
                e[i] = true;
            }
            else
            {
                pq.push(make_pair(s[i], i));
            }
        }
        string ans = "";
        for (int i = 0; i < n; ++i)
        {
            if (e[i] == true)
            {
                continue;
            }
            ans += s[i];
        }

        return ans;
    }
};
```


[3181. Maximum Total Reward Using Operations II](https://leetcode.com/problems/maximum-total-reward-using-operations-ii/description/)