# Array (Yangshun's Guide)

[Two Sum](https://leetcode.com/problems/two-sum/)

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

 

Example 1:

Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].

Example 2:

Input: nums = [3,2,4], target = 6
Output: [1,2]

Example 3:

Input: nums = [3,3], target = 6
Output: [0,1]

```python
def twoSum(nums, target):
        dicte = {}
        
        for idx in range(len(nums)):
            #create a dictionary as we go where nums are keys and indexes are values
            num = nums[idx]
            diff = target - num
            if diff in dicte.keys():
                return [dicte[diff], idx]
            else:
                dicte[num] = idx
```

[Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.


Example 1:

Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.


Example 2:

Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.

```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        cur_min = prices[0]
        max_profit = 0
        
        for num in prices:
            cur_min = min(cur_min, num)
            diff = num - cur_min
            max_profit = max(diff, max_profit)
            
        return max_profit
```

For any price to sell there is always some minimum price to buy before. The idea is to traverse the array looking for the best price to sell keeping track of (1) the minimum price to buy so far (i.e. before the currently considered price to sell) (2) maximum profit so far. As we update the minimum we consider the difference between the currently considered price to sell and the minimum so far to calculate the expected profit. If it is bigger than the max_profit we update it.

[Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

 

Example 1:

Input: nums = [1,2,3,1]
Output: true
Example 2:

Input: nums = [1,2,3,4]
Output: false
Example 3:

Input: nums = [1,1,1,3,3,4,3,2,4,2]
Output: true


```python
class Solution(object):
    def containsDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        return len(nums) != len(set(nums))

```

Solution 2 (Better O(n) time)

```python
def containsDuplicate(nums):
    nums_set = set()
    for i in nums:
        if i in nums_set:
            return True
        nums_set.add(i)
    return False
```

[Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i].

The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and without using the division operation.
 

Example 1:

Input: nums = [1,2,3,4]
Output: [24,12,8,6]

Example 2:

Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]

```python
class Solution(object):
    def productExceptSelf(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        length = len(nums)
        products = [1]*length
        
        # from left to right
        #the product at i is the product at i-1 times the number at i-1 
        for i in range(1, length):
            products[i] = nums[i-1]*products[i-1]
            
        #now we need to do the same from right to left
        right_product = 1
        for i in reversed(range(length)):
            products[i] *= right_product
            right_product *= nums[i]
            
        return products
```

[Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

A subarray is a contiguous part of an array.


Example 1:

Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.

Example 2:

Input: nums = [1]
Output: 1

Example 3:

Input: nums = [5,4,-1,7,8]
Output: 23

Solution:

We initialise the current sum with 0, and then we traverse the array with one pass. If a given element added to the current sum is smaller than 0 then that element must have been negative and we want to start summing elements from the beginning starting from its right neighbour. Else if the sum stays positive, we add the element to current sum and update the maximum sum.

We also initialise the maximum sum with the maximum element since the maximum sum of a subarray is equal or bigger than that.

```python
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
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

Alternatively:

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        mmax = nums[0] #tracks the highest sum so far
        ssum = mmax #tracks the best sum at a given point
        for i in range(1, len(nums)):
            if nums[i]+ssum > nums[i]:
                ssum = nums[i]+ssum #if the new number with what comes before is bigger than add it
            else:
                ssum = nums[i] #if the new number is bigger on its own start adding again from this number
            if ssum > mmax:
                mmax = ssum 
        return mmax
```

[Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

Given an integer array nums, find a contiguous non-empty subarray within the array that has the largest product, and return the product.

The test cases are generated so that the answer will fit in a 32-bit integer.

A subarray is a contiguous subsequence of the array.

 

Example 1:

Input: nums = [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.

Example 2:

Input: nums = [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.

Solution:

The idea is to split the problem into sub-problems to be solved iteratively/recursively as in dynamic programming. Consider the subarray containing elements from index 0 to (including) index i of the original array. Then the maximum product of a subarray (subject to the condition that it ends at index i) of this subarray is either:
1. the number at index i (in case the maximum and minimum products of subarrays ending at i-1 are negative and the number at i is positive)
2. or the number at index i mutliplied by the maximum product of a subarray ending at index i-1 (in case both are positive)
3. or the number at index i mutliplied by the minimum product of a subarray ending at index i-1 (in case both number multiplied are negative)
i.e. the maximum of the three.

Then the maximum product of a subarray of the original array is the maximum of of the maximal subarrays ending at index i across all indices i.

Similarly, we need to iteratively compute the minimum product of a subarray ending at index i, since if it is negative, multiplying it by a negative number can yield a maximum as suggested above. That is, the minimum product of a subarray ending at index i is either:
1. the number at index i (in case the maximum and minimum products of subarrays ending at i-1 are negative and the number at i is negative and smaller)
2. or the number at index i mutliplied by the maximum product of a subarray ending at index i-1 (in case the number at i is negative thus making the product negative and even smaller)
3. or the number at index i mutliplied by the minimum product of a subarray ending at index i-1 (in case the number at i is positive and the minimum product of a subarray ending at i-1 is negative)
i.e. the minimum of the three.


```python
class Solution(object):
    def maxProduct(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 0:
            return 0
        
        max_so_far = nums[0]
        min_so_far = nums[0]
        
        res = max_so_far
        
        for i in range(1, len(nums)):
            
            max_prod = max(nums[i], max_so_far*nums[i], min_so_far*nums[i])
            min_so_far = min(nums[i], max_so_far*nums[i], min_so_far*nums[i])
            max_so_far = max_prod
            
            res = max(max_so_far, res)
            
        return res
```

[Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

[4,5,6,7,0,1,2] if it was rotated 4 times.
[0,1,2,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.

Solution:

To achieve O(log n) time we need a binary search-like algorithm. We use two pointers, hi and lo, corresponding to the left and right boundaries of the subarray considered. If at any point the number at index lo is smaller than the number at index hi then this means that this subarray is sorted, hence we return lo as the index of the smallest element. Otherwise, if the number at index lo is bigger than the number at index hi, this subarray is rotated so we consider its two subarrays – from lo to mid, and from mid+1 to hi. If the number at mid is smaller than the number at lo the left subarray is not sorted and hence contains the minmum, so we move hi to mid to only consider this subarray. Otherwise the left subarray is sorted in ascending order, so the turning point must be in the right subarray.



```python
class Solution(object):
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        lo = 0
        hi = len(nums)-1
        
        while hi >= lo:
            mid = (hi+lo)//2 + 1 
            
            if nums[lo] <= nums[hi]: #then the subarray is sorted i.e. not rotated
                return nums[lo]
            
            else: #the subarray is rotated i.e. not sorted
                
                if nums[mid] < nums[lo]: #leftmost subarray not sorted, so min must be on the left
                    hi = mid
                    lo += 1
                else: #leftmost subarray sorted, but the full array not, so min must be on the right
                    lo = mid+1
```

Runtime: 24 ms, faster than 83.87% of Python online submissions for Find Minimum in Rotated Sorted Array.
Memory Usage: 13.5 MB, less than 85.42% of Python online submissions for Find Minimum in Rotated Sorted Array.

[Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

 

Example 1:

Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
Example 2:

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
Example 3:

Input: nums = [1], target = 0
Output: -1
 

Constraints:

1 <= nums.length <= 5000
-10^{4} <= nums[i] <= $10^{4}$
All values of nums are unique.
nums is an ascending array that is possibly rotated.
-$10^{4}$ <= target <= $10^{4}$

Solution:

This is similar to the problem above, but now we need to be able to locate any element in the array rather than just the minimum. To do that, we first find the rotation index i.e. the index of the smallest element of the rotated array in a similar way as above. Then we perform binary search in the appropriate subarray. If the rotation index is 0 then the whole array is sorted in ascending order so we perform binary search on the entire array. Otherwise the array is rotated, so if target is smaller than the number at index 0, it must be towards the end of the array i.e. after the rotation index so we search in the subarray starting at rotation index, and if the target is bigger than the number at index 0 it must come after it but before the rotation index so we search the appropriate subarray.

```python
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        #first find the rotation pivot index i.e. index of the smallest number
        
        def find_rotation_index(lo, hi):
            
            if nums[lo] < nums[hi]:
                return 0
            
            while lo <= hi:
                
                mid = (lo + hi)//2 
                
                if nums[mid] > nums[mid+1]: #then this is where the ascending order is violated
                    return mid + 1
                
                else:
                    
                    if nums[mid] < nums[lo]:
                    #then smallest number is in the left subarray since the left subarray is not strictly ascending
                        hi = mid - 1
                        
                    else: 
                     #then left subarray is strictly ascending so it must be on the right   
                        lo = mid + 1
                        
        def binary_search(lo, hi): #on sorted subarray
            
            while lo <= hi:
                mid = (lo+hi)//2
                
                if nums[mid] == target:
                    return mid
                else:
                    if target < nums[mid]:
                        hi = mid - 1
                    else:
                        lo = mid + 1
            return -1
        
        l = len(nums)
        
        if l == 1:
            
            if nums[0] == target:
                return 0
            else:
                return -1
            
        rotation_index = find_rotation_index(0, l - 1)
        
        if nums[rotation_index] == target:
            return rotation_index
        
        if rotation_index == 0:
            return binary_search(0, l-1)
        
        if target < nums[0]: #then it must be towards the end
            return binary_search(rotation_index, l-1)
        
        return binary_search(0, rotation_index)
```

[3Sum](https://leetcode.com/problems/3sum/)

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.

 

Example 1:

Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]

Example 2:

Input: nums = []
Output: []

Example 3:

Input: nums = [0]
Output: []

```python
def threeSum(nums):
        ans = []
        nums.sort() #sorted array is easier to work with!
        if len(nums) < 3:
            return ans
        
        for i in range(len(nums) - 2):
            if i > 0 and nums[i] == nums[i-1]: continue
            l, r = i + 1, len(nums) - 1 #first and last number
            while l < r :
                s = nums[i] + nums[l] + nums[r]
                if s == 0:
                    ans.append([nums[i] ,nums[l] ,nums[r]])
                    l += 1
                    r -= 1
                    while l < r and nums[l] == nums[l - 1]: #if the same num
                        l += 1
                    while l < r and nums[r] == nums[r + 1]: 
                        r -= 1
                        
                elif s < 0 :
                    l += 1 #since it is sorted we know that l<r so if sum <0 we need to increase l 
                    
                else:
                    r -= 1 #if sum too big we need to decrease r
        return ans
```
[Container with Most Water](https://leetcode.com/problems/container-with-most-water/)

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

Example 1:

Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

Example 2:

Input: height = [1,1]
Output: 1

Solution:

We use two pointers, left and right, corresponding to the leftmost and rightmost ends of the container. The goal is to maximise the distance between numbers while maximising the minimum of the two numbers at the leftmost and rightmost ends, and hence to maximise the area. We need an efficient way of traversing the array. We start with pointers at the leftmost and rightmost ends of the array, and with every iteration of the while loop we move one of the pointers closer to the middle – the one that points to a smaller number since we are looking to maximise these numbers. Therefore, the smaller number can be rejected immediately and other neighboring numbers can be explored. Every time we do that we calculate the area and update the maximum area.



```python
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        n = len(height)
        if n < 2:
            return 0
        
        left = 0
        right = n-1
        area = 0
        max_area = 0
        
        while left < right:
            area = (right-left)*min(height[left], height[right])
            max_area = max(max_area, area)
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1
                
        return max_area
```

[Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

 

Example 1:

Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
Example 2:

Input: nums = [1], k = 1
Output: [1]

```python
class Solution(object):
    def maxSlidingWindow(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        res = []
        n = len(nums)
        
        if n * k == 0:
            return []
        if k == 1:
            return nums
        
        deq = deque()
        pos = 0
        
        for i in range(0, min(k, n)):
            if deq and deq[0] == i-k:
                deq.popleft()
            while deq and nums[i] > nums[deq[-1]]:
                deq.pop()    
            deq.append(i)
            if nums[i] > nums[pos]:
                pos = i 

        res.append(nums[pos])
   
        for i in range(k, n):
            if deq and deq[0] == i-k:
                deq.popleft()
            while deq and nums[i] > nums[deq[-1]]:
                deq.pop()    
            deq.append(i) #to the right
            res.append(nums[deq[0]])  
            
        return res
```

Solution:

We use Python's dequeue i.e. Doubly Ended Queue, to store and update the maximum elements in the sliding window. 

We first iterate over the initial window and enqueue the element in decreasing order i.e. in the first example, we enqueue 1, then we pop 1 and enqueue 3 since it is bigger than 1, then we enqueue -1, thus ending up with a queue containing 3 and -1. This is because we are not interested in the elements on the left of the maximum (because they can't potentially become maxima of the next sliding windows), but we are interested in the elements on the left of the maximum because they can be maxima of the next sliding windows. Meanwhile, we store the index of the maximum, and we store the maximum of the first sliding window in the result array.

Then we process the remaining elements. Now we update the dequeue as we move the sliding windows, popping elements that are not longer in the sliding window, and elements that are to the left of the current maximum and smaller than it. Elements are appended on the right end, and the maximum should be at the leftmot end. Once we have the maximum at the head of the dequeue we add it to the result array. 

