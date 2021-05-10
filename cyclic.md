# Cyclic replacements

[Rotate Array](https://leetcode.com/problems/rotate-array/)

Given an array, rotate the array to the right by k steps, where k is non-negative.

Example 1:

Input: nums = [1,2,3,4,5,6,7], k = 3

Output: [5,6,7,1,2,3,4]

Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

Example 2:

Input: nums = [-1,-100,3,99], k = 2

Output: [3,99,-1,-100]

Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]

Follow up:

Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
Could you do it in-place with O(1) extra space?

```python

def rotate(nums):
        """
        Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        k = k % n
        start_loop = 0 #we start at index 0
        count = 0 #number of replacements so far
        while count < n: #we need n replacements in total
            current_idx = start_loop #starting idx for a loop
            current_num = nums[current_idx]
            while True: 
                next_idx = (current_idx + k) % n #we jump from an element to the element we want to replace 
                next_num = nums[next_idx] #we save the next element as a temporary var so that 
                #it doesn't disapper when we replace it
                nums[next_idx] = current_num #replaced
                current_num = next_num #we do the same thing for the number just replaced
                current_idx = next_idx
                count += 1     
                if current_idx == start_loop: #when we come back the starting point terminate the loop
                    break
            start_loop += 1 #if the loop came back to the starting element and we have't replaced all elements,
            #we need to start a new loop from the neighbouring element
            
            #Example of conjugate classes

```