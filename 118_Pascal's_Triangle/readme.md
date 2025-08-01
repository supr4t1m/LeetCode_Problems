118\. Pascal's Triangle
=======================

## Intuition
To generate Pascal's triangle, we need to understand that each element in the triangle is derived from the sum of the two elements directly above it. The first and last elements of each row are always 1. This leads to a straightforward way to build the triangle row by row.

## Approach
1. **Initialization**: Start with the first row of Pascal's triangle, which is simply `[1]`.
2. **Iterate through rows**: For each subsequent row, initialize a new vector with a size equal to the current row index plus one (to account for the first and last elements being 1).
3. **Set the first and last elements**: Assign the first and last elements of the current row to 1.
4. **Fill in the middle elements**: For each element in the middle of the row, calculate its value by summing the two elements directly above it from the previous row.
5. **Store the current row**: After constructing the current row, add it to the list of rows.
6. **Repeat**: Continue this process until the desired number of rows is generated.

## Complexity
- **Time complexity**: The time complexity is $$O(n^2)$$, where $$n$$ is the number of rows. This is because we are generating each row based on the previous row, and the number of elements in each row increases linearly.
  
- **Space complexity**: The space complexity is also $$O(n^2)$$, as we are storing all the rows of Pascal's triangle in a 2D vector.

## Code
```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ans(1, vector<int>(1, 1));

        int row = 1;

        while (row < numRows) {
            vector<int> curr(row+1, 0);
            curr[0] = 1; curr[row] = 1;

            for (int i=1; i<row; i++) {
                curr[i] = ans[row-1][i-1] + ans[row-1][i];
            }

            ans.push_back(curr);
            row++;
        }

        return ans;
    }
};
```
