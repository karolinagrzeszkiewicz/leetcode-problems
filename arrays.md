# Arrays

[Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

Note:

The number of elements initialized in nums1 and nums2 are m and n respectively.
You may assume that nums1 has enough space (size that is equal to m + n) to hold additional elements from nums2.
Example:

Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]

```python
def merge(nums1, m, nums2, n):
        """
        Do not return anything, modify nums1 in-place instead.
        """
        i, j = 0, 0 #counter for indexes of nums1 and nums2 respectively
        for j in range(n): #for each element in nums2 we look for a place to insert it 
            #- i.e. we insert it on the left of the first equal or bigger number in nums1
            while i < m+j and nums1[i] < nums2[j]: #m+j is the num of non-zero elements in nums1 for given j
                i += 1
            nums1.insert(i, nums2[j]) #insert at index i and move the rest of elements if nums1 by 1 to the right
            if i == m+j: #when i goes beyond then break
                break
        nums1[m+j+1:] = nums2[j+1:]
```

[Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

Given an array of integers, find if the array contains any duplicates.

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

Example 1:

Input: [1,2,3,1]
Output: true
Example 2:

Input: [1,2,3,4]
Output: false
Example 3:

Input: [1,1,1,3,3,4,3,2,4,2]
Output: true

Solution 1

```python
def containsDuplicate(nums):
        nums_set = set(nums)
        return len(nums) != len(nums_set)

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

[Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the absolute difference between i and j is at most k.

Example 1:

Input: nums = [1,2,3,1], k = 3
Output: true
Example 2:

Input: nums = [1,0,1,1], k = 1
Output: true
Example 3:

Input: nums = [1,2,3,1,2,3], k = 2
Output: false

Solution 1

```python
def containsNearbyDuplicate(nums, k):
        idx_diff = {}
        for idx, num in enumerate(nums):
            if num not in idx_diff.keys():
                idx_diff[num] = idx
            else:
                idx_diff[num] = idx - idx_diff[num]
                if idx_diff[num] <= k:
                    return True
        return False

```

Solution 2

```python

def containsNearbyDuplicate(nums, k):
        last_idx = {} 
        for idx, num in enumerate(nums):
            if num in last_idx.keys() and idx - last_idx[num] <= k:
                return True
            last_diff[num] = idx
        return False

```

[Contains Duplicate III](https://leetcode.com/problems/contains-duplicate-iii/)

Given an array of integers, find out whether there are two distinct indices i and j in the array such that the absolute difference between nums[i] and nums[j] is at most t and the absolute difference between i and j is at most k.

 

Example 1:

Input: nums = [1,2,3,1], k = 3, t = 0
Output: true
Example 2:

Input: nums = [1,0,1,1], k = 1, t = 2
Output: true
Example 3:

Input: nums = [1,5,9,1,5,9], k = 2, t = 3
Output: false

```python
def containsNearbyAlmostDuplicate(nums, k, t):
        if t == 0 and len(set(nums)) == len(nums):
            # Quick response for t = 0
             return False
            
        size = len(nums)
        
        bucket = {} #dictiobary containing buckets
        
        bucket_width = t + 1
        
        for idx, num in enumerate(nums):
            
            bucket_idx = num // bucket_width #floor division to get buckets 0, 1, 2, 3: [0, t+1], [t+2, 2t+3]
            
            if bucket_idx in bucket:
                # means there is already a number in this bucket with this idx
                #so gap between numbers must be smaller than width <= t
                return True
            
            elif bucket_idx + 1 in bucket and abs(num - bucket[bucket_idx + 1]) < bucket_width:
                
                #means there is a bucket on the right and so there is a number in it
                # if the difference between that number and our number is < width <= t:
                return True
            
            elif bucket_idx - 1 in bucket and abs(num - bucket[bucket_idx - 1]) < bucket_width:
                
                # as above, there is a neighbouring bucket on the left
                return True
            
            # add the number to its bucket
            bucket[bucket_idx] = num
            
            
            if idx >= k: #delete buckets and start with new
                #sliding window – each time we add a bucket we delete a bucket containing numbers
                #more than k indexes away (behind) from our current index
                
                # delete old number whose index distance larger than k
                del bucket[ nums[idx-k] //bucket_width ]
                
        return False

```

[Two Sum](https://leetcode.com/problems/two-sum/)

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

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

[Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

Given an array nums of n integers where n > 1,  return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

Example:

Input:  [1,2,3,4]
Output: [24,12,8,6]
Constraint: It's guaranteed that the product of the elements of any prefix or suffix of the array (including the whole array) fits in a 32 bit integer.

Note: Please solve it without division and in O(n).

Follow up:
Could you solve it with constant space complexity? (The output array does not count as extra space for the purpose of space complexity analysis.)

Solution 1 

```python
def productExceptSelf(nums):
        length = len(nums)
        left = [0]*length #for a given index product of the numbers to the left of it
        right = [0]*length #for a given index product of the numbers to the right of it
        left[0] = 1
        right[length-1] = 1
        for l in range(1, length):
            left[l] = left[l-1]*nums[l-1]
        for r in reversed(range(length-1)):
            right[r] = right[r+1]*nums[r+1]
            r -= 1
        products = []
        for i in range(length):
            products += [left[i]*right[i]]
        return products

```

Solution 2 

For O[1] space complexity set products to be the 'products to the left' and then iterate through nums and products once again this time multiplying products[i] by a variable R tracking the 'product to the right':

```python
def productExceptSelf(nums):
        length = len(nums)
        products = [0]*length
        
        products[0] = 1
        for i in range(1, length):
            products[i] = nums[i-1]*products[i-1]
            
        right_product = 1
        for i in reversed(range(length)):
            products[i] = products[i]*right_product
            right_product *= nums[i]
            
        return products

```

[Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

Example 1:

Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
Example 2:

Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.

```python
def maxProduct(nums):

```

[Find Minimum in a Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

[4,5,6,7,0,1,2] if it was rotated 4 times.
[0,1,2,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums, return the minimum element of this array.

 

Example 1:

Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
Example 2:

Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
Example 3:

Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times. 

Solution 1

```python 
def findMin(nums):
        n = len(nums)
        i = 1
        while i < n and nums[i] > nums[i-1]:
            i +=1
        idx = i % n 
        return nums[idx]
```

Solution 2 (Binary Search)

```python 
def findMin(nums):
        n = len(nums)
        #look for inflection point with binary search
        if len(nums) == 1:
            return nums[0]
        
        #pointers
        left = 0
        right = len(nums)-1
        
         #if the last element is greater than the first element then there is no rotation
        if nums[right] > nums[left]:
            return nums[left]   
        
        while right >= left:
            
            mid = left + (right-left)//2
            
            # if the mid element is greater than its next element then mid+1 element is the smallest
            if nums[mid] > nums[mid + 1]:
                return nums[mid + 1] #inflection point from higher to lower value
            
            if nums[mid -1] > nums[mid]:
                return nums[mid] #inflection point from higher to lower value
            
            # if the mid elements value is greater than the 0th element then
            # the least value must be somewhere to the right
            
            if nums[mid] > nums[0]:
                left = mid + 1
                
            #if not then it must be to the left
            else:
                right = mid - 1
            
```

[3Sum](https://leetcode.com/problems/3sum/)

Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

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
                    r -= 1 #if sum too big we need to increase r
        return ans
```
