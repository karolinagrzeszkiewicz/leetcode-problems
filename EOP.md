# EOP questions for 1 month

[Reverse Bits](https://leetcode.com/problems/reverse-bits/submissions/)
  ```python
  class Solution:
      def reverseBits(self, n):
          res, power = 0, 31
          while n:
              res += (n & 1) << power
              n = n >> 1
              power -= 1
          return res
  ```

Because binary numbers are represented through 2 being raised to power x and we are dealing with 32-bit unsigned integer, we start with power 31.
```n & 1``` is compares the right-most bit in n to 1 and if they are both 1 returns 1. This is done to see whether the leftmost bit of the final number is going to be 1 or 0.
``` << power ``` is the number of bits we want to shift the result of ```n & 1 ``` to the left.
```n >> 1``` reduces n by 1 bit from the right. E.g. 33 will be reduced to 16
