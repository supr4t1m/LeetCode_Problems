# Intuition
The key insight to solve this problem is to observe that the maximum height that a building can be increased to is the minimum of the maximum height in its row and the maximum height in its column. This is because the skyline from any cardinal direction must not change, which means that the height of each building cannot exceed the maximum height in its row or column.

# Approach
1. Iterate through the grid and find the maximum height in each row and store it in a `rowMax` vector.
2. Iterate through the grid and find the maximum height in each column and store it in a `colMax` vector.
3. Iterate through the grid again and for each building, calculate the difference between the minimum of the maximum height in its row and the maximum height in its column, and the building's current height. Add this difference to the `ans` variable.
4. Return the `ans` variable as the maximum total sum that the height of the buildings can be increased by without changing the city's skyline from any cardinal direction.

# Complexity
- Time complexity: $$O(m \times n)$$, where `m` is the number of rows and `n` is the number of columns in the grid. We iterate through the grid three times, each time taking $$O(m \times n)$$ time.
- Space complexity: $$O(m + n)$$, where `m` is the number of rows and `n` is the number of columns in the grid. We use two additional vectors, `rowMax` and `colMax`, each of size `m` and `n` respectively.

# Code
```cpp
class Solution {
public:
    int maxIncreaseKeepingSkyline(vector<vector<int>>& grid) {
        vector<int> rowMax;
        vector<int> colMax;

        // get max values for each row
        for (int i=0; i<grid.size(); i++) {
            int max=0;
            for (int j=0; j<grid[0].size(); j++) {
                if (grid[i][j] > max) max = grid[i][j];
            }
            rowMax.push_back(max);
        }

        // get the max values for each column
        for (int j=0; j<grid[0].size(); j++) {
            int max = 0;
            for (int i=0; i<grid.size(); i++) {
                if (grid[i][j] > max) max = grid[i][j];
            }
            colMax.push_back(max);
        }

        int ans=0;
        for (int i=0; i<grid.size(); i++) {
            for (int j=0; j<grid[0].size(); j++) {
                ans += min(rowMax[i], colMax[j]) - grid[i][j];
            }
        }

        return ans;
    }
};
```
