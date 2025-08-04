904\. Fruit Into Baskets
========================

# Intuition

<!-- Describe your first thoughts on how to solve this problem. -->

We need to find the longest subarray that contains at most two distinct types of fruits. This is essentially a classic **"Longest Substring with at Most Two Distinct Characters"** problem, which can be solved using a **sliding window** approach. The idea is to keep extending the window to the right as long as there are no more than two fruit types, and shrink the window when a third type is added.

---

# Approach

<!-- Describe your approach to solving the problem. -->

1. Traverse the `fruits` array from left to right.
2. Use a `set` to keep track of distinct fruit types in the current window.
3. Maintain a `count` of the number of fruits picked in the current valid window.
4. If more than two types are detected (i.e., `set.size() > 2`):

   * Clear the set and reset it with the last two fruit types.
   * Move backwards from the current index to find how far the last repeated fruit continues.
   * Reset `count` accordingly to start a new valid window from that point.
5. Keep track of the maximum number of fruits picked (`ans`) at each step.

**Note:** This code doesn't use a classic sliding window with two pointers and a map or frequency counter. Instead, it uses a set and backtracking, which is functional but not the most efficient or standard way to solve this.

---

# Complexity

* **Time complexity:**
  $O(n)$
  In the worst case, each element is visited at most twice — once during the forward traversal and possibly once more when backtracking during a reset.

* **Space complexity:**
  $O(1)$
  The set can hold at most 3 elements temporarily, so the space used is constant.

---

# Code

```cpp []
class Solution {
public:
    int totalFruit(vector<int>& fruits) {
        int n = fruits.size();
        set<int> s;
        int count = 0;
        int ans = 0;

        // iterate through the array
        for (int i = 0; i < n; i++) {
            s.insert(fruits[i]); // insert current fruit type
            count++;

            // if a third type is encountered
            if (s.size() > 2) {
                s.clear(); // clear the set
                int j = i - 1;
                s.insert(fruits[i]);
                s.insert(fruits[j]);

                // step backward to count how far the last fruit repeats
                while (j > 0 && fruits[j] == fruits[j - 1]) 
                    j--;
                count = i - j + 1; // reset count to the new window size
            }

            // update max fruits picked
            if (count > ans)
                ans = count;
        }

        return ans;
    }
};
```
