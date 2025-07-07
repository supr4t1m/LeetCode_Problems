LC594. Longest Harmonious Subsequence
=====================================

# Intuition
<!-- Describe your first thoughts on how to solve this problem. -->
The problem requires finding a subsequence where the maximum and minimum values differ by exactly 1. The key insight is to use a frequency-based approach or a sliding window technique to efficiently find such a subsequence.

# Approach
<!-- Describe your approach to solving the problem. -->
The approach involves:
1. Sorting the array to group similar elements together
2. Using a two-pointer technique to slide through the array
3. Tracking the length of subsequences where the difference between max and min is exactly 1
4. Keeping track of the maximum such subsequence length

# Code
```C++ []
class Solution {
public:
    int findLHS(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        int len = 0;

        sort(nums.begin(), nums.end());

        int left = 0, right = 0;

        while (right<n) {
            if (nums[right] - nums[left] < 1) right++;
            else if (nums[right] - nums[left] > 1) left++;
            else {
                len = right-left+1;
                right++;
            }

            if (len > ans) ans = len;
        }

        return ans;
    }
};
```

# Complexity
- Time complexity: $$O(n \log n)$$
  - Sorting the array takes $$O(n \log n)$$ time
  - The two-pointer traversal takes $$O(n)$$ time

- Space complexity: $$O(1)$$ 
  - Only using a constant amount of extra space for pointers and tracking variables
  - The sorting is typically done in-place for most sorting algorithms
