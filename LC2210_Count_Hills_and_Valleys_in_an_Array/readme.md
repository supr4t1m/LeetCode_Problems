2210\. Count Hills and Valleys in an Array
=========================================

# Intuition
To solve the problem of counting hills and valleys in the given array, we need to identify the indices that meet the criteria for being a hill or a valley. A hill is characterized by having neighbors that are both smaller than the current element, while a valley has neighbors that are both larger. The challenge lies in efficiently finding these neighbors while also handling cases where adjacent elements are equal.

# Approach
1. **Iterate through the array**: We will loop through the array starting from the second element and ending at the second-to-last element, as these are the only indices that can have both left and right neighbors.
  
2. **Skip equal neighbors**: If the current element is equal to its left neighbor, we skip to the next iteration since it cannot be a hill or valley.

3. **Find non-equal neighbors**: For each valid index, we will look for the closest non-equal neighbors on both sides. This is done using two while loops that decrement and increment indices until we find a different value or reach the boundaries of the array.

4. **Check conditions for hills and valleys**: After identifying the left and right neighbors, we check if the current element is greater than both neighbors (indicating a hill) or less than both neighbors (indicating a valley). If either condition is satisfied, we increment our count.

5. **Return the count**: Finally, we return the total count of hills and valleys found.

# Complexity
- Time complexity: $$O(n^2)$$  
  We traverse the array once, and in the worst case, we may need to check each element's neighbors, but this is still linear in terms of the number of elements.

- Space complexity: $$O(1)$$  
  We use a constant amount of space for variables, regardless of the input size.

# Code
```cpp []
class Solution {
public:
    int countHillValley(vector<int>& nums) {
        int ans = 0;
        int n = nums.size();

        for (int i=1; i<n-1; i++) {
            
            // part of the same hill or valley
            if (nums[i-1] == nums[i])
                continue;

            int j = i-1; 
            int k = i+1;

            // find the left and right neighbour
            while (nums[i] == nums[j] && j>0) j--;
            while (nums[i] == nums[k] && k<n-1) k++;

            // check for hill
            if (nums[i] > nums[j] && nums[i] > nums[k]) ans++;

            // check for valley
            else if (nums[i] < nums[j] && nums[i] < nums[k]) ans++;
        }

        return ans;
    }
};
```
