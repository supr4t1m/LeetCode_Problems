2044\. Count Number of Maximum Bitwise-OR Subsets
================================================

# Intuition

<!-- Describe your first thoughts on how to solve this problem. -->

We need to find the number of **non-empty subsets** whose **bitwise OR** is equal to the **maximum possible bitwise OR** of any subset. The key idea is:

* First, determine the **maximum bitwise OR** achievable using all elements.
* Then, use **backtracking** to explore all possible subsets and **count** how many of them achieve this maximum.

# Approach

<!-- Describe your approach to solving the problem. -->

1. **Compute the maximum OR**: Loop through the `nums` array and perform a bitwise OR on all elements. This gives us the target `max_or`.
2. **Use backtracking** to generate all possible subsets. For each subset, we:

   * Include the current number (OR it with the running result).
   * Or skip the current number (do not include it).
3. At each leaf of the recursion (when we’ve processed all elements), if the subset's OR equals `max_or`, we count it.
4. The recursive `backtrack` function returns the count of subsets with `max_or`.

# Complexity

* **Time complexity:**
  $O(2^n)$
  We explore all subsets (each element is either included or not), leading to $2^n$ combinations.

* **Space complexity:**
  $O(n)$
  Due to the recursion stack in the backtracking approach.

# Code

```cpp
class Solution {
public:
    int countMaxOrSubsets(vector<int>& nums) {
        int max_or = 0;

        for (int i: nums)
            max_or = max_or | i;

        return backtrack(0, max_or, 0, nums);
    }

    int backtrack(int running_or, int target_or, int index, vector<int>& nums) {
        // at the end if the subset matches maximum or
        // include it
        if (index == nums.size())
            return running_or == target_or?1:0;

        // either include the number or don't 
        return backtrack(running_or | nums[index], target_or, index+1, nums) + backtrack(running_or, target_or, index+1, nums);
    }
};
```

This solution explores all subsets recursively and counts how many reach the desired maximum OR value.
