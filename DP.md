# Dynamic Programming

DP is a technique used to solve optimization problems in the most efficient way. We break down a problem into smaller sub-problems and compute the solution to each such problem only once which guarantees lower time complexity and possibly memory usage than if we were to solve that problem recursively.

Memoization is perhaps the most important technique in DP. With memoization there is no re-computation - each sub-problem is solved only once. 

For example, the problem below asks us to compute the number of ways we can climb n stairs by climbing 1 or 2 stairs at a time. The key insight is to notice that number of ways to climb n stairs = number of ways to climb n-1 stairs (then we just add 1 step) + number of ways to climb n-2 stairs (then we add 2 stairs). And the same holds for n-2 and n-1 so our computation is recursive following the pattern of Fibonacci numbers - we need to solve n-1 sub-problems before. But we don't need to go backward and forward like in recursion computing the solution to each such sub-problem 3 times. Instead we can start from the first two fibonacci numbers and compute one new fibonacci number at a time based only on the last two numbers 'in our memory' until we reach the nth number - so we compute a solution to a sub-problem by using memorized solutions to the last two sub-problems.

We can use memoization array to store solutions to sub-problems in the right order corresponding to dependency relations between solutions. Size equal to the number of sub-problems on which our problem relies.


[Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

 

Example 1:

Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
Example 2:

Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

Solution 1
  ```python
  def climbStairs(self, n: int) -> int:
        if n==1:
            return 1
        elif n==2:
            return 2
        else:
            fibonacci = [0,1,2]
            for i in range(3,n+1):
                fibonacci.append(fibonacci[i-1]+fibonacci[i-2])
            return fibonacci[-1]
  ```

Solution 2
 ```python
  def climbStairs(self, n: int) -> int:
        last = 0
        current = 1
        for i in range(1, n+1):
            temp = current
            current = last + current
            last = temp
        return current
  ```


[Coin Change](https://leetcode.com/problems/coin-change/)

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

 

Example 1:

Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
Example 2:

Input: coins = [2], amount = 3
Output: -1
Example 3:

Input: coins = [1], amount = 0
Output: 0
Example 4:

Input: coins = [1], amount = 1
Output: 1
Example 5:

Input: coins = [1], amount = 2
Output: 2

Solution
  ```python
  def coinChange(coins, amount):
        coins.sort()
        F = [float('inf')] * (amount + 1)
        F[0] = 0
        
        for coin in coins:
            for x in range(coin, amount + 1):
                F[x] = min(F[x], F[x - coin] + 1)
        if F[amount] != float('inf'):
            return F[amount]  
        else:
            return -1
  ```

Explanation:

Let F(S) be the minimum number of coins as change for amount S. Then, in general F(S) = F(S-c) + 1, where c is the value of a particular coin belonging to change. Yet, since we don't know c we take F(S) = min(F(S - Ci)) + 1, where Ci are coins in the set.

Knowing this relationship we construct the F array from bottom up for all S in the range [0, amount] to eventually obtain F(amount).

We know that F(0) = 0. 

Then for each coin we compute F(S) for all S between the value of the coin and the amount. As we do that for different coins the value F(i) for given i becomes update to always be the minimum across coin for which we made computations so far. 

If in the end of this process F(amount) still has its intitial value (very big number) this means that given amount cannot be changed for a sum of given coins as index amount of F cannot be reached by adding coins we have. 
  
[Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

Given an integer array nums, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, [3,6,2,7] is a subsequence of the array [0,3,1,6,2,2,7].

 

Example 1:

Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
Example 2:

Input: nums = [0,1,0,3,2,3]
Output: 4
Example 3:

Input: nums = [7,7,7,7,7,7,7]
Output: 1
```python
def lengthOfLIS(nums):
        if len(nums) == 0:
            return 0
        memo = [0]*len(nums)
        memo[0] = 1 
        max_global = 1 
        for i in range(1, len(nums)):
            max_until_idx_i = 0 
            for j in range(i):
                if nums[i] > nums[j]:
                    max_until_idx_i = max(max_until_idx_i, memo[j])
            memo[i] = max_until_idx_i + 1
            max_global = max(max_global, memo[i])
        
        return max_global
```

Explanation:

We create a memoization array memo which stores the length of a longest increasing subsequence until and including index i. 

Base case: until and including index 0 the longest subsequence has length 1 (the number at index 0 is the subsequence)

Next case: Let 0<j<i be the last number before i such that nums[i] is bigger than nums[j]. Then the length of the largest subsequence until index smaller or equal to j. So the length of the longest increasing subsequence until and including i is the length of the longest increasing subsequence until i plus 1 (corresponding to adding nums[i] to the sequence).

As we iterate through nums and memo we update the global max to be the highest memo value so far. 


[Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Example 1:

Input: nums = [-2,1,-3,4,-1,2,1,-5,4]

Output: 6

Explanation: [4,-1,2,1] has the largest sum = 6.

Example 2:

Input: nums = [1]

Output: 1

Solution 1
```python
def maxSubArray(nums):
        mmax = nums[0] #tracks the highest sum so far, strictly increasing
        ssum = mmax #tracks the best sum at a given point, fluctuates
        for i in range(1, len(nums)):
            if nums[i]+ssum > nums[i]:
                ssum = nums[i]+ssum #if the new number with what comes before is bigger than add it
            else:
                ssum = nums[i] #if the new number is bigger on its own start adding again from this number
            if ssum > mmax:
                mmax = ssum 
        return mmax
```

Solution 2

```python 
def maxSubArray(nums):
        cur_sum = 0
        max_sum = max(nums)
        for i in range(len(nums)):
            if cur_sum + nums[i] < 0:
                cur_sum = 0
            else:
                cur_sum = cur_sum + nums[i]
                max_sum = max(max_sum, cur_sum)
        return max_sum
```

Explanation:

As we traverse the list:

We use ssum to keep track of the best sum at a given point and initialize it as the first element. Thus ssum is defined recursively as either the previous sum with the new element added (if this new ssum is greater than the current elemement) or the new element (if this element is bigger on its own than added to the previous sum).

We use the variable mmax to keep track of the highest sum encountered so far, initialising it to the first ssum, i.e. the first element. As we traverse the list obtaining successive ssums, if an ssum is bigger than the current mmax, we set mmax to that ssum. In this manner, mmax is strictly increasing and the final mmax is the biggest sum encountered so far.


[Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

Example 1:

Input: [7,1,5,3,6,4]

Output: 5

Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5. Not 7-1 = 6, as selling price needs to be larger than buying price.

```python
def maxProfit(prices):
        n = len(prices)
        if n < 1:
            return 0
        else:
            local_min = prices[0]
            max_profit = 0
            for p in prices[1:n]:
                if p < local_min:
                    local_min = p
                else:
                    max_profit = max(max_profit, p - local_min)
            return(max_profit)
```

Explanation:

This problem can be reduced to finding the minimum predecessor for each number in the list (going from left to right) and then returning the biggest difference between a number and its minimal predecessor.

[Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

Say you have an array prices for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

Example:

Input: [7,1,5,3,6,4]

Output: 7

Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4. Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.

```python
def maxProfit(prices):
        i = 0
        price_min = prices[0]
        price_max = prices[0]
        max_profit = 0
        while (i < len(prices)-1): #continue unntil we cover all indexes 
            while (i < len(prices)-1 and prices[i] >= prices[i+1]): #stop when we reach the lowest point
                i += 1
            price_min = prices[i]
            while (i < len(prices)-1 and prices[i] <= prices[i+1]): #stop when we reach the peak
                i += 1
            price_max = prices[i]
            max_profit += price_max - price_min #the difference between local max and min is the profit
        return max_profit
```


[House Robber](https://leetcode.com/problems/house-robber/)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

```python
def rob(nums):
        if len(nums) > 1:   
            sums = [nums[0], max(nums[1], nums[0])]
            for i in range(2, len(nums)):
                max_so_far = max(sums[i-1], sums[i-2]+nums[i])
                sums.append(max_so_far)
            return(sums[len(nums)-1])
        else:
            return (nums[0])
```

We use a memoization array called sums to keep track of the maximum amount of money that can be robbed until and including house i. So the last element of the array will ne the maximum amount that can be robbed from all houses given the constraints.

Then for house i the maximum amount that can be robbed until and icnluding that house is either the maximum amount for house i-1 (since if house i-1 is robbed house i can't be robbed) or the maximum amount for house i-2 plus the amount in house i, depending on which one of them is bigger. 

