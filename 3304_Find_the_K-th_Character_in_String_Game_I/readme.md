LC3304. Find the K-th Character in String Game I
================================================

# Intuition
The problem involves generating a character based on the number of set bits in the binary representation of k-1. This is an interesting approach that uses bit manipulation to determine how many times we should move to the next character in the alphabet.

# Approach
The algorithm works by:
1. Decrementing k by 1 to adjust the indexing
2. Counting the number of set bits in the binary representation of k
3. Using the count of set bits to determine the final character

Key steps in the bit manipulation:
- Use k%2 to check if the least significant bit is set
- If the bit is set (k%2 is true), increment the answer counter
- Divide k by 2 (right shift) to move to the next bit
- Repeat until k becomes 0
- Use modulo 26 to wrap around the alphabet

# Example Walkthrough
Let's trace the algorithm for k = 5:
1. k becomes 4 (k-1)
2. Binary representation of 4 is 100
3. Bit counting process:
   - 4 ÷ 2 = 2 (binary 100)
     - Least significant bit is 0, ans = 0
   - 2 ÷ 2 = 1 (binary 10)
     - Least significant bit is 0, ans = 0
   - 1 ÷ 2 = 0 (binary 1)
     - Least significant bit is 1, ans = 1
4. Result: 'a' + 1 = 'b'

# Complexity
- Time complexity: $$O(\log k)$$
  - The while loop runs for the number of bits in k
  - Each iteration divides k by 2

- Space complexity: $$O(1)$$
  - Uses only a constant amount of extra space
  - Only stores a single integer (ans)

# Code Explanation
```cpp
class Solution {
public:
    char kthCharacter(int k) {
        int ans = 0;  // Counter for number of set bits

        // Adjust indexing by subtracting 1
        k--;

        // Count set bits
        while (k) {
            // If least significant bit is 1, increment ans
            if (k%2) ans++;
            
            // Right shift (divide by 2)
            k = k/2;
        }

        // Return character by adding ans to 'a'
        return char('a' + ans%26);
    }
};
```
