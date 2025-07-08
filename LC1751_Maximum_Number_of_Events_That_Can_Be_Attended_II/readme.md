LC1751. Maximum Number of Events That Can Be Attended II
========================================================

# Intuition
The problem requires finding the maximum value of events that can be attended, with a constraint of attending at most k events. This suggests using dynamic programming with a recursive approach that explores different event selection strategies.

# Approach
The solution uses a depth-first search (DFS) with memoization approach:
1. Sort events by start time to enable efficient processing
2. Use a 2D DP table to memoize results
3. For each event, have two choices:
   - Skip the current event
   - Attend the current event and move to the next non-overlapping event
4. Use binary search (bisectRight) to find the next non-overlapping event efficiently
5. Recursively explore all possible event combinations while respecting the k-event limit

# Complexity
- Time complexity: $$O(n \log n \cdot k)$$
  - Sorting events: $$O(n \log n)$$
  - DFS with memoization: $$O(n \cdot k)$$
  - Binary search for next event: $$O(\log n)$$
  - Overall: $$O(n \log n \cdot k)$$

- Space complexity: $$O(n \cdot k)$$
  - DP memoization table: $$O(n \cdot k)$$
  - Recursion stack: $$O(\log n)$$

# Key Algorithmic Techniques
1. Dynamic Programming with Memoization
2. Recursive DFS
3. Binary Search for efficient event selection
4. Event sorting for optimal processing

The solution elegantly handles the constraints by:
- Limiting event attendance to k
- Ensuring no event overlap
- Maximizing total event value

The binary search method `bisectRight` is particularly clever in finding the next non-overlapping event efficiently, which helps reduce the overall time complexity.

# Code
```cpp []
class Solution {
public:
    vector<vector<int>> dp;
    int maxValue(vector<vector<int>>& events, int k) {
        int n = events.size();
        dp = vector<vector<int>>(k+1, vector<int>(n, -1)); // k+1 x n dp memo table

        // sort the events in increasing order of their start times 
        sort(events.begin(), events.end(), [](vector<int> e1, vector<int>e2) -> bool {
            return e1[0] < e2[0];
        });

        return dfs(0, k, events);
    }

    int dfs(int curr_index, int count, vector<vector<int>>& events) {
        /* we don't get any value going beyond k events
         * or exploring past all the events
         */
        if (count == 0 || curr_index == events.size())
            return 0;

        if (dp[count][curr_index] != -1) 
            return dp[count][curr_index];

        int next_index = bisectRight(events, events[curr_index][1]);

        /* explore the two possibilities, excluding the current event and
         * moving on to the next event, or including the current event and 
         * getting the value from it and move to the next event that can be
         * attended found with binary search
         */
        return dp[count][curr_index] = max(dfs(curr_index+1, count, events), 
                        events[curr_index][2] + dfs(next_index, count-1, events));
    }

    int bisectRight(vector<vector<int>>& events, int target) {
        /* setting the search space from very first event to the
         * end of the last event (n), since we might have to 
         * insert the current event at the end of the array
         */
        int left = 0, right = events.size();

        while (left < right) {
            int mid = left + (right-left)/2;

            /* in this binary search, once the loop breaks
             * left will be the minimum key that will
             * NOT obey the condition 
             * (events[mid][0] <= target), while right will
             * always disobey the condition
             */
            if (events[mid][0] <= target) {
                left = mid+1;
            } else {
                right = mid;
            }
        }

        return left;
    }
};
```
