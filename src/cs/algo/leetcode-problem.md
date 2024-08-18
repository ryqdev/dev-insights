# Leetcode Problem Solutions

```cpp
// 1. two sum
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> m;
        int idx = 0;
        for (auto i : nums) {
            if (m.find(target - i) != m.end()) {
                return {idx, m[target - i]};
            }
            m[i] = idx++;
        }
        return {0, 0};
    }
};
```

```cpp
// 2. Add Two Numbers

/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int c = 0;
        ListNode* dummy = new ListNode();
        ListNode* cur = dummy;
        while (l1 || l2 || c) {
            int v1 = l1 ? l1 -> val : 0;
            int v2 = l2 ? l2 -> val : 0;
            int new_v = (v1 + v2 + c) % 10;
            c = (v1 + v2 + c) / 10;
            ListNode* new_node = new ListNode(new_v);
            cur -> next = new_node;
            cur = cur -> next;
            l1 = l1 ? l1 -> next : l1;
            l2 = l2 ? l2 -> next : l2;
        }
        return dummy -> next;
    }
};

```

```cpp
// 3. Longest Substring Without Repeating Characters
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int ans = 0;
        int n = s.size();
        unordered_map<int , bool> m;
        int left = 0;
        for (int i = 0; i < n; ++i) {
            while(m.find(s[i]) != m.end()) {
                m.erase(s[left++]);
            }
            m[s[i]] = true;
            ans = max(ans, i - left + 1);
        }
        return ans;
    }
};
```

