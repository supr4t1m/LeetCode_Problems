2106\. Maximum Fruits Harvested After at Most K Steps
=====================================================

## Intuition
The problem involves maximizing the number of fruits harvested from various positions on an infinite x-axis, given a starting position and a maximum number of steps. The key insight is to consider the positions of the fruits relative to the starting position and how far we can move in either direction. By calculating how many fruits can be harvested from both the left and right sides of the starting position, we can determine the optimal strategy for movement.

## Approach
1. **Prefix Sums**: We will create two arrays, `left` and `right`, to store the cumulative number of fruits that can be harvested when moving left and right, respectively. The index of these arrays will represent the number of steps taken from the starting position.

2. **Filling the Arrays**: 
   - For each fruit position, if it lies within the range of steps we can take to the left (from `startPos` to `startPos - k`), we update the `left` array. 
   - Similarly, if the fruit position is within the range of steps to the right (from `startPos` to `startPos + k`), we update the `right` array.

3. **Calculating Prefix Sums**: After populating the `left` and `right` arrays, we compute the prefix sums for both arrays. This allows us to quickly determine the total number of fruits that can be harvested for any given number of steps.

4. **Maximizing Harvest**: We then evaluate four scenarios:
   - Harvesting only from the left.
   - Harvesting only from the right.
   - Harvesting from the left first and then the right.
   - Harvesting from the right first and then the left.
   For each scenario, we keep track of the maximum number of fruits harvested.

5. **Return the Result**: Finally, we return the maximum number of fruits harvested from all scenarios.

## Complexity
- **Time complexity**: $$O(n + k)$$, where $$n$$ is the number of fruit positions and $$k$$ is the maximum number of steps. We iterate through the fruit positions once and then perform operations proportional to $$k$$.
  
- **Space complexity**: $$O(k)$$, as we use two arrays of size $$k + 1$$ to store the prefix sums for left and right movements.

## Code
```cpp
class Solution {
public:
    int maxTotalFruits(vector<vector<int>>& fruits, int startPos, int k) {
        vector<int> left(k+1, 0), right(k+1, 0);

        for (const auto& p: fruits) {
            // fruits at start position (if any) are included in the left
            // prefix sum
            if (p[0] <= startPos && p[0] >= startPos - k)
                left[startPos - p[0]] = p[1];
            else if (p[0] > startPos && p[0] <= startPos + k)
                right[p[0] - startPos] = p[1];
        }

        // calculate prefix sum
        for (int i=1; i<=k; i++) {
            left[i] = left[i-1] + left[i];
            right[i] = right[i-1] + right[i];
        }

        int ans = 0; 

        // pure left case
        for (int i=1; i<=k; i++) {
            if (left[i] > ans)
                ans = left[i];
        }

        // pure right case
        for (int i=1; i<=k; i++) 
            if (right[i] > ans)
                ans = right[i];

        // mixed: left and then right case
        for (int i=0; 2*i<=k; i++) {
            int tot = left[i] + right[k - 2*i];
            if (tot > ans)
                ans = tot;
        }

        // mixed: right and then left case
        for (int i=0; 2*i<=k; i++) {
            int tot = right[i] + left[k - 2*i];
            if (tot > ans)
                ans = tot;
        }

        return ans;
    }
};
```
