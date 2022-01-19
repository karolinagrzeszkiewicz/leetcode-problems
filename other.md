# Miscellaneous Problems

[Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)

Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements of [1, n] inclusive that do not appear in this array.

Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.

```python

def findDisappearedNumbers(nums):
        n = len(nums) 
        integers = [0]*n #correspond to counts of integers 1, 2, ..., n
        disappeared = []
        for i in nums:
            integers[i - 1] = 1
        for i in range(n):
            j = integers[i]
            if j == 0:
                disappeared.append(i+1)
        return disappeared

```

[Search Insert Position](https://leetcode.com/problems/search-insert-position/)

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

Solution 1
```python
def searchInsert(nums, target):
        i = 0
        while i < len(nums) and nums[i] < target:
            i += 1
        return i
```
Solution 2 (Binary Search)

```python
def searchInsert(nums, target):
        l, r = 0, len(nums)-1
        
        while l <= r:
            
            mid = l + (r - l)//2 #midpoint between left and right
            
            if nums[mid] > target: #if mid bigger than target
                #then target must be to the left of mid so max mid-1 so set right = mid - 1
                r = mid - 1
                
            elif nums[mid] < target: #if middle number is smaller than target then target 
                #must be on the right so at least mid+1 so set left = mid + 1
                l = mid + 1
                
            else:
                return mid
            #if target is the middle number return the index
        
        return l #if we get to the point where l > r (because target is greater than current middle 
    #number but smaller than the right neighbour of the middle number) then return left 
    #i.e. middle index + 1 (by last iteration of the while loop l = mid + 1)
```

[First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

Given a string, find the first non-repeating character in it and return its index. If it doesn't exist, return -1.

Examples:

s = "leetcode"
return 0.

s = "loveleetcode"
return 2.
 

Note: You may assume the string contains only lowercase English letters.

Solution 1 (Hash table)

```python
def firstUniqChar(s):
        #using dictionaries
        if s == "":
            return -1
        else:
            dict = {}
            n = len(s)
            for i in range(n):
                char = s[i]
                if char not in dict.keys():
                    dict[char] = [1, i]
                else:
                    dict[char][0] += 1
            print(dict)
            for num in dict.values():
                if num[0] == 1:
                    return num[1]
            return -1

```

Solution 2 (using collections.counter which stores elements as keys and their counts as values in a dictionary)

```python
import collections

def firstUniqChar(s):
        counter = collections.Counter(s) #creates a hash map
        #find the first character with count 1 and its index
        for idx, char in enumerate(s): #enumerate (iterable object, start = 0)
            if counter[char] == 1:
                return idx
        return -1
```

[Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.

```python
def isValid(s):
        dict = {
            "(": ")",
            "{": "}",
            "[": "]"
        }
        from collections import deque
        stack = deque()
        for elem in s:
            if elem in dict.keys():
                stack.append(elem)
            else:
                if stack == deque([]):
                    return (False)
                else:
                    pair = stack.pop()
                    print(pair)
                    if elem == dict[pair]:
                        continue
                    else:
                        return (False)
        return (stack == deque([]))
```


[Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)

Given an array of meeting time intervals intervals where intervals[i] = [starti, endi], return the minimum number of conference rooms required.

Example 1:

Input: intervals = [[0,30],[5,10],[15,20]]

Output: 2

Example 2:

Input: intervals = [[7,10],[2,4]]

Output: 1

Solution 1 (Priority Queue)

The idea is to sort intervals by start time, so that we can allocate rooms in the order of start times. Then we store end times in a min heap i.e. with the smallest element at the top, such that the heap only stores the meetings that are 'currently happening'. Then we can pop the meeting end times for the meetings that have ended and push the meeting end times for the meetings that have started. 

Then when we want to allocate a room for a meeting happening between start time and end time we check if its start time is bigger than the end time on top of the heap (i.e. smallest) – if it is then we pop that element thereby terminating the meeting and push an updated end time thus starting a new meeting in that same room, if it is not then we simply push the end time thereby allocating a new room.

```python
class Solution(object):
    def minMeetingRooms(self, intervals):
        """
        :type intervals: List[List[int]]
        :rtype: int
        """
        if len(intervals) == 0:
            return 0
        
        #initialise the heap with end times as keys
        free_rooms = []
        
        #sort the meeting in increasing order of start time
        intervals.sort(key = lambda x : x[0])
        
        #push the first end time to free_rooms heap
        heapq.heappush(free_rooms, intervals[0][1])
        
        for interval in intervals[1:]:
            
            #if the room on top is free take it
            if free_rooms[0] <= interval[0]:
                heapq.heappop(free_rooms)
                
            #add the updated ending time to the heap
            heapq.heappush(free_rooms, interval[1])
            
        return len(free_rooms)
```

Solution 2 (Chronology and Pointers)

Instead of considering intervals (which cannot have a total order), we can consider the start and end timings in ascending order which gives us chronology of start and end timings. Once we have start timings sorted, we can traverse them trying to allocate a new or used room to each like we did in the previous approach, and once we have the end timings sorted we can launch and terminate meetings by considering whether their end times are before or after the start of the meeting currently considered. This is similar to the min heap used in the approach above, which always had the earliest ending time of a meeting at the top but now instead of popping it we just move the pointer to the next minimum ending time.


```python
class Solution(object):
    def minMeetingRooms(self, intervals):
        """
        :type intervals: List[List[int]]
        :rtype: int
        """
        if len(intervals) == 0:
            return 0
        
        start_times = sorted(interval[0] for interval in intervals)
        end_times = sorted(interval[1] for interval in intervals)  
        
        rooms_taken = 0
        max_rooms_taken = 0
        
        start_pointer = 0
        end_pointer = 0
        
        while start_pointer < len(intervals):
            
            if start_times[start_pointer] < end_times[end_pointer]:
                
                #means we need to allocate another room
                
                rooms_taken += 1
                start_pointer += 1
                
            else:
                
                #means that we can schedule the meeting after the meeting that has    ended (since the start and end times are sorted this ending meeting is the meeting ending the earliest before the starting meeting)
                
                #finish this meeting
                end_pointer += 1 
                
                #allocate this toom to new meeting
                #rooms_taken stays constant
                
                #move to the next meeting 
                start_pointer +=1
                
        return rooms_taken
```

