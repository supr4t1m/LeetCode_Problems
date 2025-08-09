# Intuition
<!-- Describe your first thoughts on how to solve this problem. -->
We can use bitwise XOR to check whether the bits of the number zero out with pure power of 2. Any additional bits other than that at exponent will not be filtered. 

# Approach
<!-- Describe your approach to solving the problem. -->
For every exponent from 0 to 31, we do bitwise XOR of the number and power. In case any other bits are left in the number, it is not a power of 2. 

# Complexity
- Time complexity: $$O(1)$$
Even though we are iterating through the exponent, we are doing so for a constant number of times (31 times). 
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: $$O(1)$$
We are only using variables which take up constant space complexity. 
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```cpp []
class Solution {
public:
    bool isPowerOfTwo(int n) {

        for (int i=0; i<32; i++) {
            if (n > 0 && !(n ^ 1<<i))
                return true;
        }

        return false;
    }
};
```
