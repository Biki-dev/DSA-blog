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
## Approach 2: Sorting

**Idea:**  
Sort the array first. Since the majority element appears more than `n/2` times, after sorting it will occupy a continuous block larger than half the array. We traverse the sorted array and count consecutive occurrences of each element. As soon as any element's frequency exceeds `n/2`, we return it.

**Time Complexity:** O(n log n)  
**Space Complexity:** O(1)

### C++ Code with Line-by-Line Comments

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();                    // Step 1: Get the size of the array
        
        sort(nums.begin(), nums.end());         // Step 2: Sort the array in ascending order
                                                // After sorting, all same elements come together
        
        int freq = 1;                           // Step 3: Initialize frequency counter to 1 (current element appears at least once)
        int ans = nums[0];                      // Step 4: Assume first element is the answer initially
        
        // Step 5: Traverse the sorted array from second element
        for (int i = 1; i < n; i++) {
            
            if (nums[i] == nums[i - 1]) {       // If current element is same as previous one
                freq++;                         // Increase the frequency count
            } 
            else {                              // If current element is different from previous
                freq = 1;                       // Reset frequency to 1 for the new element
                ans = nums[i];                  // Update ans to the new current element
            }
            
            if (freq > n / 2) {                 // If frequency of current element exceeds n/2
                return ans;                     // Then it is the majority element, return it
            }
        }
        
        return ans;                             // If loop ends, the last element must be the majority
                                                // (guaranteed by problem statement)
    }
};
```

---