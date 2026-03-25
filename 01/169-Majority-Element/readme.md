# 169. Majority Element

## Problem Statement
Given an array `nums` of size `n`, return the **majority element**.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element **always exists** in the array.

### Examples

**Example 1:**
```
Input: nums = [3,2,3]
Output: 3
```

**Example 2:**
```
Input: nums = [2,2,1,1,1,2,2]
Output: 2
```

---

## Approach 1: Brute Force

**Idea:**  
For each element in the array, count its frequency by traversing the entire array again. If any element's frequency is greater than `n/2`, return it.

**Time Complexity:** O(n²)  
**Space Complexity:** O(1)

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();           // size of the array
        
        for (int val : nums) {         // take each value one by one
            int freq = 0;              // reset frequency counter
            
            for (int el : nums) {      // nested loop to count occurrences
                if (el == val) {
                    freq++;
                }
            }
            
            if (freq > n / 2) {        // check if it is majority element
                return val;
            }
        }
        
        return -1;                     // though problem guarantees majority element exists
    }
};
```

---
