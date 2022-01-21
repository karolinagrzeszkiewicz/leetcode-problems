[Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)

Given two integers a and b, return the sum of the two integers without using the operators + and -.

Example 1:

Input: a = 1, b = 2
Output: 3
Example 2:

Input: a = 2, b = 3
Output: 5
 

Constraints:

-1000 <= a, b <= 1000

Solution:

We have to consider different cases: is a positive? is b positive? Is a greater than b? To simplify it we require that the absolute value of a is bigger or equal to the absolute value of b, and if it is not we swap the numbers. Then we need to know whether the numbers are positive or negative so we consider 2 cases: (1) both are positive or negative, in which case we can add their absolute values |a|+|b| and multiply the result by the sign in the end (2) one is positive and one negative in which case what we want to calculate is |a|-|b| (which is positive since |a| is bigger) multiplied by the sign of a. 

Case (1): |a|+|b|
This is the standard addition in columns but in base-2 system (rather than base-10) but we want to use the logical bitwise operations. Applying XOR to two binary representations does addition in columns but without the remainder that should be carried over to the bits on the left i.e. 1 XOR 1 will return 0 but in this case 1 should be carried over and XORed with the next bits on the left. But we can get a binary representations corresponding to this remainder by ANDing the two numbers which will yield 1 bits where both numbers have the 1 bit. Then this has to be shifted to the left by 1 since we want it to be carried over to the next additions on the right. And then guess what? We can XOR the remainder obtained this way with the result of the XOR to "add" them. But then there will be a remainder again. So we do this iteratively.

Case (2): |a|-|b|
Now this is subtraction in columns with the bigger number on top (which simplifies a lot!). Likewise, we can XOR the numbers since 1 - 1 = 0 and 1 XOR 1 = 0, 1 - 0 = 1 and 1 XOR 0 = 1, 0 - 0 = 0 and 0 XOR 0 = 0... Yet 0 - 1 = -1 and 0 XOR 1 = 0. So in this case we have a remainder, but a negative one unlike in addition. There will be a remainder every time the upper number has a 0 bit (so the negation of it is 1) and the lower number has 1.
Therefore, we can detect these bits with (NOT X) AND Y. Then we have to shift them to the left, and subtract from the result of XOR. And so it goes.


```python
class Solution:
    def getSum(self, a: int, b: int) -> int:
            
        x = abs(a)
        y = abs(b)
        
        # x >= y required
        
        if x < y:
            return self.getSum(b, a)  
        
        sign = 1 if a > 0 else -1
        
        if a * b >= 0:
            # both positive or both negative
            # so calculate the sum of two positive integers and then multiply by sign
            
            while y:
                #apply XOR
                #add the remainder ie bits where both x and y have 1 resulting in 1, shoft to the left
                x, y = x ^ y, (x & y) << 1
                
        else:
            # one positive one negative so assume x >= y and multiply x-y by sign
            # to calculate difference of two positive integers with x >= y:
            
            while y:
                #apply XOR
                #subtract the remainder ie bits where x has 0 and y has 1 resulting in -1, shift to the left
                x, y = x ^ y, ((~x) & y) << 1
        
        return x * sign
```

[Number of 1 bits](https://leetcode.com/problems/number-of-1-bits/)

Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).

Note:

Note that in some languages, such as Java, there is no unsigned integer type. In this case, the input will be given as a signed integer type. It should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 3, the input represents the signed integer. -3.
 

Example 1:

Input: n = 00000000000000000000000000001011
Output: 3
Explanation: The input binary string 00000000000000000000000000001011 has a total of three '1' bits.
Example 2:

Input: n = 00000000000000000000000010000000
Output: 1
Explanation: The input binary string 00000000000000000000000010000000 has a total of one '1' bit.
Example 3:

Input: n = 11111111111111111111111111111101
Output: 31
Explanation: The input binary string 11111111111111111111111111111101 has a total of thirty one '1' bits.
 

Constraints:

The input must be a binary string of length 32.
 

Follow up: If this function is called many times, how would you optimize it?

Solution 1:

We can simply iterate over the bits and increment the result by the current bit each time. Instead of iterating over 32 bits every time, we can calculate the number of bits the number actually takes up (excluding the 000..00 prefix) by taking the ceiling of log base 2 of the input number and incrementing it by 1. Then for i in that range we find the ith bit by ANDing the number with the binary number consiting of 0s and a single 1 as the ith bit from the right, which will return a number with 1 at the ith position and 0s elsewhere if and only if the binary representation of our number has 1 at the ith bit from the right. In Python, this will be the ith power of 2 (or otherwise 0) so we divide it by 2 to the power of i to get our bit.


```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        
        res = 0 
        
        if n == 0:
            return 0
        
        l = ceil(math.log2(n))+1
        
        for i in range(l):
            res += (n & (1 << i))//2**i
            
        return res
```

Solution 2:

Ideally we would like to iterate only over the 1s in the binary representation. Yet how to do that? 

We can use the fact that ANDing a number n with n-1 always removes the rightmost 1 from the binary representation of n (and does not change any other bits). Tthis is because if n has the rightmost 1 bit at the ith position (and 0s at positions 0 to i-1) then n-1 will have a 0 at the ith position (corresponding to 2 to the power of i) but instead it will have have 1s at positions 0 to i-1. So the bits from i to 0 are pairwise negations of each other and hence ANDing them will yield 0s. 

Now every time we do that a single 1 bit disappears from the binary representation of our number. So we can do that iteratively, incremeting our result by 1 (corresponding to the 1 bit hat just disappeared from the number), until our number becomes 0s all the way.

```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        res = 0
        
        while n:
            res += 1
            n &= n-1
            
        return res
```

[Counting Bits](https://leetcode.com/problems/counting-bits/)

Given an integer n, return an array ans of length n + 1 such that for each i (0 <= i <= n), ans[i] is the number of 1's in the binary representation of i.


Example 1:

Input: n = 2
Output: [0,1,1]
Explanation:
0 --> 0
1 --> 1
2 --> 10
Example 2:

Input: n = 5
Output: [0,1,1,2,1,2]
Explanation:
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101
 

Constraints:

0 <= n <= 105
 

Follow up:

It is very easy to come up with a solution with a runtime of O(n log n). Can you do it in linear time O(n) and possibly in a single pass?
Can you do it without using any built-in function (i.e., like __builtin_popcount in C++)?

Solution 1:

In order to check how many 1-bits a number has we can simply keep subtracting powers of 2 that are smaller than our number and each subtracted number will correspond to a 1 bit. Note that we have to skip the powers that are bigger than our current number (these will correspond to 0s in the binary representation).

```python
class Solution:
    def countBits(self, n: int) -> List[int]:
        
        num = 0
        
        res = [0]
        
        for i in range(1, n+1):
            
            #geatest power of 2 smaller than i
            p = floor(math.log2(i)) 
            num = i
            counter = 0
            
            while num > 0:
                if num >= 2**p:
                    num -= 2**p
                    counter += 1
                p -= 1
            
            res.append(counter)
            
        return res
```

Solution 2 (Dynamic Programming):

The problem with the solution above is that for each number it performs subtraction of all powers of 2 smaller than it, leading to O(nlogn) complexity. This procedure is unncecessarily repetitive since e.g. 15 - 8 = 7 so the number of 1-bits of 15 is 1 (corresponding to 8) plus the number of 1-bits of 7, which we already know assuming we are traversing the numbers in ascending order. Then rather than enumerating all powers of 2 each time we could reuse the solutions for preceding numbers:

```python
class Solution:
    def countBits(self, n: int) -> List[int]:
        
        res = [0]*(n+1)
        
        for i in range(1, n+1):
            
            #greatest power of 2 smaller than i
            p = floor(math.log2(i))
            
            res[i] = 1 if i - 2**p == 0 else (1 + res[i - 2**p])                
            
        return res
```

Now this algorithm has O(n) time complexity since for each number it performs a constant time operations of accessing a single element of array and writing to a single index of the array.

Solution 3 (Dynamic Programming):

Alternatively, we can use the relationship between i and i//2. Every number i has either the same number of digits as i//2 (if i is even e.g. 7 = (111) and 14 = (1110)) or the number of digits of i//2 plus 1 (if i is odd e.g. 15 = (1111) and 7 = (111)):


```python
class Solution:
    def countBits(self, n: int) -> List[int]:
        
        res = [0] * (n + 1)
        
        for i in range(1, n + 1):
            
            # i // 2 is i >> 1 and i % 2 is i & 1
            res[i] = res[i >> 1] + (i & 1) 
            
        return ans 
```

Solution 4 (Dynamic Programming):

Final trick: i & i-1 has the same binary representation as i but without the least significant 1-bit (this isnight was used in the previous question)
so the number of 1-bits of i is the same as the number of 1-bits of i&(i-1) plus 1 (to compensate for the removed 1-bit).

```python
class Solution:
    def countBits(self, n: int) -> List[int]:
        
        res = [0] * (n + 1)
        
        for i in range(1, n + 1):
            res[i] = res[i & (i - 1)] + 1
            
        return res
```

