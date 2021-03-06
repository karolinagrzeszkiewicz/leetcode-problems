# Arrays and strings

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

[Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

Given a string s, find the length of the longest substring without repeating characters.


Example 1:

Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
Example 2:

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
Example 3:

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
Example 4:

Input: s = ""
Output: 0

```python
def lengthOfLongestSubstring(s) :
        left = 0
        right = 0
        max_len = 0 
        chars_used = {}
        while right < len(s):
            char = s[right]
            if char in chars_used.keys() and left <= chars_used[char]: #means that both repetitions are in the window
                left = chars_used[char] + 1
            else: #if not in the hash table or outside of the window 
                max_len = max(max_len, right-left+1)
            chars_used[char] = right #update the hashtable with most recent index
            right += 1
        return max_len
```

We use a sliding window defined by the index left on the left and right on the right and a hash table to keep track of the most recent index of a given character.

Each time we access a character at the right end of the window:

If the character is already in the hash table and if it is within our window (on the right of the left index) then we move the left end of the window to the following character.

Else if the rightmost character is not in the sliding window we keep going to the right and update the maximum length of the window.

In any case, we update the most recent index of the rightmost character in the hash table and move to the next character.

[Container with Most Water](https://leetcode.com/problems/container-with-most-water/)

Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of the line i is at (i, ai) and (i, 0). Find two lines, which, together with the x-axis forms a container, such that the container contains the most water.

Notice that you may not slant the container.

Example 1:

Input: height = [1,8,6,2,5,4,8,3,7]

Output: 49

Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

Example 2:

Input: height = [1,1]

Output: 1

Example 3:

Input: height = [4,3,2,1,4]

Output: 16

Example 4:

Input: height = [1,2,1]

Output: 2

Solution 

```python
def maxArea(height):
        left = 0
        right = len(height)-1
        max_area = (right-left)*min(height[left],height[right])
        while left < right:
            area = (right-left)*min(height[left],height[right])
            max_area = max(max_area, area)
            if height[right] < height[left]:
                right -= 1
            else:
                left += 1
        return (max_area)
```

Explanation:

We use a sliding window defined by left and right pointers which start at the leftmost and rightmost ends of the array and gradually move inwards computing the area defined by the window. The main source of optimization is the way in which we move the pointers towards each other: we look at the height corresponding to the left and right pointers, and we choose to move towards the centre the pointer with the lower height. This is because a bigger height might be worth keeping in the window since it might keep the minimum of the two heights high.

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

Explanation:

We use a hash table to keep track of the elements already traversed and their indices. As we traverse the array for each element we find the difference between the target and that element and check if there is a number in the hash table corresponding to that difference. If there is such number we know its index from the hash table so we return the pair of indices. If a number is not in the hash table we add it to the hash table. 

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

Solution 1

```python

def threeSum(nums):
        
        def twosum(nums, i, res):
            left = i + 1
            right = len(nums)-1
            while left < right:
                ssum = nums[i] + nums[left] + nums[right]
                if ssum > 0:
                    right -= 1
                elif ssum < 0:
                    left += 1
                else:
                    res.append([nums[i], nums[left], nums[right]])
                    left += 1
                    right -= 1
                    while left < right and nums[left] == nums[left-1]:
                        left += 1
                    
        res = []
        nums.sort()
        for i in range(len(nums)):
            if nums[i] > 0:
                break
            if i == 0 or nums[i-1] !=  nums[i]:
                twosum(nums, i, res)
        return res

```
 
 Explanation: 
 
 We want to traverse the elements of the array and for each element num we want to find if there is a pair of numbers that sum up to -num using the twosum function (its complements).
 We assume that the element traversed is negative and its complements are positive. Hence, we sort the array initially which will allow us to optimise the traversal by traversing only negative elements in the outer loop. It doesn;t affect the complexity since sorting is done in O(n^2) and we're aiming at O(n^2)  complexity overall. Also in a sorted array identical elements are next to each other, and so if there is a repetition we can easily skip it.
 
 In the inner two sum function called on negative non-repeated elements and always the first element of the list we use two pointers initialised as the leftmost and rightmost boundary of the window on the right of the number accessed in outer loop. We traverse the array with the pointers moving towards the centre and each time we compute the sum of the number from the outer loop, the left pointer and the right pointer. If the sum is 0 we append it to the result and move both pointers towards the centre (since then it is impossible to get a 0 by changing only one number in the sum). If the sum is bigger than 0 we want the sum to decrease, so we move the right pointer to the left i.e to smaller numbers. If the sum is smaller than 0 then we want it to increase so we move the left pointer to the right.
 
 
Solution 2

```python

def threeSum(nums) :
        
        def twosum2(nums, i, res):
            traversed_elems = set()
            j = i + 1
            while j < len(nums):
                complement = -nums[i] - nums[j]
                if complement in traversed_elems:
                    res.append([nums[i], nums[j], complement])
                    while j < len(nums)-1 and nums[j] == nums[j+1]:
                        j += 1
                traversed_elems.add(nums[j])
                j += 1  
                                
        res = []
        nums.sort()
        for i in range(len(nums)):
            if nums[i] > 0:
                break
            if i == 0 or nums[i-1] !=  nums[i]:
                twosum2(nums, i, res)
        return res

```

This solution also relies on sorting. However, this time as we traverse the array from left to right in the outer loop for each such number we traverse all numbers to the right of it and for each such pair of numbers we check if their complement is in the set where the set contains the numbers already traversed in the inner loop. Even though we use a set lookup in O(1) the complexity is still O(n^2) since we do two array/subarray traversals.
 

 [Next Permutation](https://leetcode.com/problems/next-permutation/)

 Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such an arrangement is not possible, it must rearrange it as the lowest possible order (i.e., sorted in ascending order).

The replacement must be in place and use only constant extra memory.

 

Example 1:

Input: nums = [1,2,3]
Output: [1,3,2]
Example 2:

Input: nums = [3,2,1]
Output: [1,2,3]
Example 3:

Input: nums = [1,1,5]
Output: [1,5,1]
Example 4:

Input: nums = [1]
Output: [1]

Solution

```python

def nextPermutation(nums):
        """
        Do not return anything, modify nums in-place instead.
        """
        #find pivot

        i = len(nums)-2
        while i >= 0 and nums[i+1] <= nums[i]:
            i -= 1

        if i < 0:
            #means the nums are strictly decreasing so no greater permutation

            for j in range(0, len(nums)//2):
                j_swap = len(nums)-1-j
                nums[j], nums[j_swap] = nums[j_swap], nums[j]
        else:
            pivot_idx = i
            pivot = nums[pivot_idx]

            #find next biggest

            next_biggest, next_biggest_idx = nums[pivot_idx + 1], pivot_idx + 1
            for k in range( pivot_idx + 2, len(nums), 1):
                if nums[k] <= next_biggest and nums[k] > pivot:
                    next_biggest, next_biggest_idx = nums[k], k
            #swap the pivot with next biggest

            nums[pivot_idx], nums[next_biggest_idx] = nums[next_biggest_idx], nums[pivot_idx]
            window_len = len(nums) - (pivot_idx + 1)

            #swapping

            for j in range(pivot_idx+1, pivot_idx + 1 + window_len//2, 1):
                j_swap = len(nums)-1 - (j-(pivot_idx+1))
                nums[j], nums[j_swap] = nums[j_swap], nums[j]

```

Explanation:

Imagine the successive numbers in the array form a single number e.g. [1, 2, 3] is 123. Then finding the next permutation is equivalent to finding the next biggest number than can be constructed from these digits i.e. in the above case 132 i.e [1, 3, 2].

To do that we need to find the pivot i.e. traversing teh array from right to left the first number such that number is smaller than its neighbour on the right so that we have a decreasing subsequence. Having such number we know that there is potential to get the next permutation since that means that there are bigger numbers on the right of that number and we can swap that number with them thus getting a bigger permutation. 

Yet we want to find the next bigger permutation, so we want to swap the pivot with the next biggest number on its right (rather than any bigger number) and then all that is left is sorting the numbers on the right of the pivot index which requires only reversing that subarray since the numbers in it are decreasing (since they are on the right of the pivot). We can reverse that subarray by swapping in place – first element from the left with the first from the right, second from the left with second from the right etc. until we reach the middle.

If on the other hand the numbers in the array are strictly decreasing (there is no pivot) then this is the biggest permutation so we can't find a next permutation. Hence, we sort them in ascending order i.e. reverse the array by swapping in place as outlined above.

[Rotate Image](https://leetcode.com/problems/rotate-image/)

You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

Example 1:

Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]

Example 2:

Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]

Example 3:

Input: matrix = [[1]]
Output: [[1]]

Example 4:

Input: matrix = [[1,2],[3,4]]
Output: [[3,1],[4,2]]

Solution 

```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        n = len(matrix[0])
        layers = n // 2
        
        for l in range(layers):
        
            for i in range(l, n-1-l):

                elems = [(l, i), (i, n-1-l), (n-1-l, n-1-i), (n-1-i,l)]

                (row, col) = elems[-1]
                tmp = matrix[row][col]

                for elem in elems:
                    (row, col) = elem
                    tmp2 = matrix[row][col]
                    matrix[row][col] = tmp
                    tmp = tmp2

```

Explanation

We can think of an nxn square matrix as having n//2 layers i.e. 'squares within squares' where if n is odd the innermost square consisting of a single number is not counting as a layer since it doesn't have to be rotated. Then we can iterate over these layers from outermost to innermost. 

For each such layer we need to perform swapping in several cycles: a cycle corresponding to the corners of the layer, a cycle corresponding to elements that are 1 element away clockwise from the corners, elements that are 2 elements clockwise from the corners etc. For each such cycle we have 4 elements where the 4th element has to be inserted in the position of the first element, the first element in the position fo the 2nd element, the 2nd element in the position of the 3rd element, the 3rd element in the position of the 4th element. we can do that by storing the elements that are removed from a position and soon to be inserted in another position as temporary variables. 


[Multiply Strings](https://leetcode.com/problems/multiply-strings/)

Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.

Note: You must not use any built-in BigInteger library or convert the inputs to integer directly.

Example 1:

Input: num1 = "2", num2 = "3"

Output: "6"

Example 2:

Input: num1 = "123", num2 = "456"

Output: "56088"


[Jump Game](https://leetcode.com/problems/jump-game/)

Given an array of non-negative integers nums, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

 

Example 1:

Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

Example 2:

Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
 

Constraints:

1 <= nums.length <= 104
0 <= nums[i] <= 105

...

Solution 1

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        n = len(nums)-1
        memo = [False]*(n+1)
        memo[n] = True
        for idx in range(n-1, -1, -1):
            max_jump = min(idx + nums[idx], n)
            for i in range(idx + 1, max_jump + 1, 1):
                if memo[i]:
                    memo[idx] = True
                    break
        return memo[0] 
```

Explanation:

We use a memoization array to keep track of whether it is possible to jump to the last position (in a single step or a sequence) from a given position. For a given index idx and corresponding furthest jump given by nums[idx] if there is an index within a reach from idx (given by the furthest jump) labelled as True in the memoization array (which means that we can get to the last position from that index) then the memoization array at idx will be True too. 

Hence the memoization array at index 0 corresponds to the statement whether we can jump to the last position from index 0 i.e. first position.

Solution 2

```python

class Solution:
    def canJump(self, nums: List[int]) -> bool:
        last_pos = len(nums) - 1 #to keep track of the leftmost good position going from right to left
        for i in range(len(nums)-1, -1, -1):
            if i + nums[i] >= last_pos: 
                last_pos = i # if we can jump from this position to the last good position or further then it is good
        return last_pos == 0
```

Explanation:

We can further optimize this by using a single variable that keeps track of the leftmost position from which we can reach the last position instead of using the memoization array. For a given index i if the index i moved by the furthest possible jump from it is at the last leftmost position or to the right from it (which means that we can reach the last leftmost position from the position given by index i) then it corresponds to the new leftmost position from which we can reach the last position.

Solution 3

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        jmax = 0
        for i, jump in enumerate(nums):
            if i > jmax:
                return False
            jmax = max(jmax, i + jump)
        return True
```

Explanation:

To optimize even more we can traverse the array from left to right and keep track of the biggest length of a jump or a sequence of jumps (jmax) so far (which should always be bigger than the current index) – if at any point the index i we are accessing is bigger than jmax that means it cannot be reached from previous indices and hence we can't reach the last index either. 
 
 [Plus One](https://leetcode.com/problems/plus-one/)
 
 Given a non-empty array of decimal digits representing a non-negative integer, increment one to the integer.

 The digits are stored such that the most significant digit is at the head of the list, and each element in the array contains a single digit.

 You may assume the integer does not contain any leading zero, except the number 0 itself.

  

 Example 1:

 Input: digits = [1,2,3]
 
 Output: [1,2,4]
 
 Explanation: The array represents the integer 123.
 
 Example 2:

 Input: digits = [4,3,2,1]
 
 Output: [4,3,2,2]
 
 Explanation: The array represents the integer 4321.
 
 Example 3:

 Input: digits = [0]
 
 Output: [1]


```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        i = len(digits) - 1
        last = digits[i]
        while i >= 0 and last == 9:
            digits[i] = 0
            i -= 1
            if i >= 0:
                last = digits[i]
        if i == -1 and last == 9:
            return([1] + digits)
        else:
            digits[i] = last + 1
        return(digits)
```

Explanation:

Primary school addition – going from right to left find the first digit that is not 9 and add1 to it. Set all of the digits to its right equal to 0. If all digits are 9s then turn them into 0s and add a 1 in front.

[Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

A substring is a contiguous sequence of characters within the string.


Example 1:

Input: s = "ADOBECODEBANC", t = "ABC"

Output: "BANC"

Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.

Example 2:

Input: s = "a", t = "a"

Output: "a"

Explanation: The entire string s is the minimum window.

Example 3:

Input: s = "a", t = "aa"

Output: ""

Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.

Solution:

```python

class Solution:
    def minWindow(self, s: str, t: str) -> str:
        
        if not t or not s:
            return ""
    
        dict_t = Counter(t)
        unique_t = len(dict_t)
        unique_window = 0 #number of unique character from t that are present in the window in their required frequency
        
        #filter characters from s into one that only contains characters that are in t
        
        s_filtered = []
        for i, char in enumerate(s):
            if char in dict_t:
                s_filtered.append((i, char))
        
        left, right = 0, 0
        
        window = {}
        
        ans = len(s)+1, None, None
        
        #iterate over right, adjust left
        while right < len(s_filtered):
            
            character = s_filtered[right][1]
            window[character] = window.get(character, 0) + 1
            
            if window[character] == dict_t[character]: #means we have the required frequency in the window
                unique_window += 1 
              
            #if the current window has all of the necessary characters save it and try to trim it from the left
            while left <= right and unique_t == unique_window: 
                
                character = s_filtered[left][1]
                
                #save the smallest window
                end_idx = s_filtered[right][0]
                start_idx = s_filtered[left][0]
                
                if end_idx - start_idx + 1 < ans[0]:
                    ans = (end_idx - start_idx + 1, start_idx, end_idx)
                
                #trim it
                window[character] -= 1
                
                if window[character] < dict_t[character]:
                    unique_window -= 1
                
                left += 1
            
            # when the window shrinks to empty or the window doesn't contains all of the required characters extend the
            #window to the right
            right += 1
            
        return "" if ans[0] == len(s)+1 else s[ans[1]:ans[2]+1]  

```
Explanation:

Since the characters in s that are not in t are irrelevant we can filter them out by creating an array that stores only the 'relevant' characters (i.e. the characters that are in t) and their indices in s.

We traverse the array with left and right pointers serving as the left and right boundaries of the sliding window. We progressively increment right to traverse the entire array and to extend the window so that it includes all of the required characters, and we increment left to trim the window to the minimal possible window that contains all of the required characters and save the minimal lengths and corresponding indices on the way.
