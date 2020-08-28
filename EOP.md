# EOP questions From Scenario 3

![Image of EOP timeline](EOP.png)

[Reverse Bits-4.3](https://leetcode.com/problems/reverse-bits/submissions/)
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


[Rectangle Overlap-4.11](https://leetcode.com/problems/rectangle-overlap/submissions/)
  ```python
  class Solution:
      def isRectangleOverlap(self, rec1: List[int], rec2: List[int]) -> bool:
          if rec1 == rec2:
              return True
          x11, y11, x12, y12 = rec1[0], rec1[1], rec1[2], rec1[3]      
          x21, y21, x22, y22 = rec2[0], rec2[1], rec2[2], rec2[3]   

          bool_x1 = (x21 in range(x11+1,x12)) or (x22 in range(x11+1,x12)) 
          bool_x2 = (x11 in range(x21+1,x22)) or (x12 in range(x21+1,x22))

          bool_y1 = (y21 in range(y11+1,y12)) or (y22 in range(y11+1,y12)) 
          bool_y2 = (y11 in range(y21+1,y22)) or (y12 in range(y21+1,y22))

          return (bool_x1 or bool_x2) and (bool_y1 or bool_y2)
  ```
  
[Remove Dupicates from Sorted Array-5.5](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
```
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if not nums:
            return 0
        idx = 1
        for i in range(1,len(nums)):
            if nums[i] != nums[idx-1]:
                nums[idx] = nums[i]
                idx += 1
        return idx
```

Keep two indices with one iterating over the list and the other one over all unique elements of the list, with each repeating element being replaced by the next unique one.


[Count Primes-5.9](https://leetcode.com/problems/count-primes/)
```
class Solution:
    def countPrimes(self, n: int) -> int:
        primes = 0
        is_prime = [False, False] + [True] * (n - 1)
        for i in range(2, n):
            if is_prime[i]:
                primes += 1
                for p in range(i, n, i):
                    is_prime[p] = False
        return primes
```
  This method is called seiving. Numbers 0 and 1 are not primes. The ```is_prime``` array is an array of numbers notifying weather the number in that index is prime. We make jumps of the number equivalent to ```i``` and label them as not prime whenever we encounter a prime.


[Letter Combinations of a phone number- 6.7](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
```
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return []
        mapping = [0, 1, "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"]
        menomic, partial_mneomic = [], [None] * len(digits)
        
        def combo_helper(digit):
            if digit == len(digits):
                menomic.append("".join(partial_mneomic))
            else:
                for char in mapping[int(digits[digit])]:
                    partial_mneomic[digit] = char
                    combo_helper(digit+1)
        combo_helper(0)
        return menomic
```

The idea is to make recursive calls each time across all combinations of a digit. There are upto 4 possible letters for each of the n digits, taking O(4^n) time for recursive calls. Each base case take O(1) time and there are n base cases, so the total time complexity is O(4^n * n).

[Count and Say- 6.8](https://leetcode.com/problems/count-and-say/)

```
class Solution:
    def countAndSay(self, n: int) -> str:
        def convert(self, n_str):
            leng = len(n_str)
            s = ""
            i = 0            
            while i < leng:
                freq = 1
                while (i + 1) <= len(n_str) - 1 and n_str[i] == n_str[i+1]:
                    freq += 1
                    i += 1
                s += str(freq) + str(n_str[i])    
                i += 1
            return s

        init = "1"
        if n == 1:
            return init
        for i in range(n-1):
            init = convert(self,init)
        return init
```

The idea is to do a string conversion each time (n-1) times. So the init is updates n-1 times until we reach the desired string. Each time the init is converted, we add the frequency of a string at index i and its values to s, eg. "One 1" refers to a frequency of 1 and value of one.

Each sucessive digit can have twice as many numbers when all digits are different. The max length can have no more than 2^n digits Since there are n digits and the work in each digit is proportional to the number computed in the iteration, a simple bound on the time complexity is O(n* 2^n).


[Odd Even Linked List- 7.10](https://leetcode.com/problems/odd-even-linked-list/submissions/)

- Brute Force Solution
```python
class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        idx = 0
        even = []
        odd = []
        while head:
            if idx % 2 == 0:
                even.append(head.val)
            else:
                odd.append(head.val)
            head = head.next
            idx += 1
        total = even + odd
        new_head = ListNode()
        copy = new_head
        for val in total:
            new_node = ListNode(val)
            new_head.next = new_node
            new_head = new_head.next
        return copy.next
```
- Space Optimized Solution from EOP
```
class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        even, odd = ListNode(0), ListNode(0)
        tails, turn = [even, odd], 0
        while head:
            tails[turn].next = head
            head = head.next
            tails[turn] = tails[turn].next
            turn ^= 1
        tails[1].next = None
        tails[0].next = odd.next
        return even.next
```

The idea is to have an even list and an odd list, and an oscillator turn that oscillates between 0 and 1 uisng the bit wise exclusive operatior ```^```. Eventually you combine the even and odd list together. 


- [Valid Parenthesis - 8.3](https://leetcode.com/problems/valid-parentheses/)
```
class Solution:
    def isValid(self, s: str) -> bool:
        mapping = {"{": "}", "(":")", "[":"]"}
        stack = []
        for char in s:
            if char in mapping.keys():
                stack.append(char)
            else:
                if len(stack) == 0 or mapping[stack.pop()] != char:
                    return False
        return (len(stack) == 0)
```

- [Implementing queues using stacks - 8.8](https://leetcode.com/problems/implement-queue-using-stacks/)

```
class MyQueue:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.stack1 = []
        self.stack2 = []
        
    def push(self, x: int) -> None:
        """
        Push element x to the back of queue.
        """
        self.stack1.append(x)

    def pop(self) -> int:
        """
        Removes the element from in front of queue and returns that element.
        """
        if len(self.stack2)!=0:
            return self.stack2.pop()
        else:
            while len(self.stack1) != 0:
                self.stack2.append(self.stack1.pop())
            return self.stack2.pop()

    def peek(self) -> int:
        """
        Get the front element.
        """
        if len(self.stack2)!=0:
            return self.stack2[-1]
        else:
            while len(self.stack1) != 0:
                self.stack2.append(self.stack1.pop())
            return self.stack2[-1]
        

    def empty(self) -> bool:
        """
        Returns whether the queue is empty.
        """
        return (len(self.stack1) == 0 and len(self.stack2) == 0)

```

The idea is to have two stacks, enqueue takes O(1) time and dequeue takes O(1) time if stack 2 is not empty, else we need to transfer the elements to stack 2 from stack 1.Its impossible to solve this problem just using one stack.


- [Binary Tree Inorder Traversal- 9.11](https://leetcode.com/problems/binary-tree-inorder-traversal/)

- Recursive Solution
    ```
    class Solution:
        def inorderTraversal(self, root: TreeNode) -> List[int]:
            return self.helper(root, [])


        def helper(self, root, acc):
            if root:
                self.helper(root.left, acc)
                acc.append(root.val)
                self.helper(root.right, acc)
            return acc
    ```
- Iterative Solution
  ```
  class Solution:
      def inorderTraversal(self, root: TreeNode) -> List[int]:
          res, stack = [], []
          while True:
              while root:
                  stack.append(root)
                  root = root.left
              if not stack:
                  return res
              node = stack.pop()
              res.append(node.val)
              root = node.right
  ```

- [Heaps Recap- read page 25/26](https://drive.google.com/file/d/1hfhhpJvuvD7VCtVAs10D8p8FMAB2tqMZ/view)

- Heaps are specialized binary trees with the property that the key at each node is atleast as great as the keys stored at its children (the opposite definition is true for min heaps). 

- A max heap is implemented as an array. The children of the node at index i are at indices (2i +1) and (2i + 2).

- ```Insertions``` - O(log```n```)
- ```Look-up for max element``` - O(1)
- ```Deletion of max element``` - O(log```n```)
- ``` Searching for Arbitiary keys``` - O(```n```) 

![Image of Heaps BootCamp](Heaps.png)
 
[Find Median from Data Stream- 10.5](https://leetcode.com/problems/find-median-from-data-stream/)

```python
from heapq import *

class MedianFinder:

    def __init__(self):
        
       # To store higher side of numbers
        self.min_heap = [] 
        
        #To store the lower side of numbers
        self.max_heap = [] 
        
    def addNum(self, num: int) -> None:
        heappush(self.max_heap, -heappushpop(self.min_heap, num))

        if len(self.max_heap) > len(self.min_heap):
            heappush(self.min_heap, -heappop(self.max_heap))



    def findMedian(self) -> float:
        
        if len(self.max_heap) == len(self.min_heap):
            return (-self.max_heap[0] + self.min_heap[0]) / 2
        else:
            return self.min_heap[0]
```

The idea is to use a min-heap and a max heap. The min heap stores the top k highest numbers and the max heap stores the rest of (k -n) numbers on the lower side of numbers. Each time you push the lowest element of the min hep into the max-heap(rememeber to use a negative sign) and always make sure that the min heap is larger than the max heap.

[Valid Perfect Squares-11.5](https://leetcode.com/problems/valid-perfect-square/submissions/)

##### Approach One
```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        i = 1
        while (num > 0):
            num -= i
            i += 2
        return num == 0
```

The idea is to reduce the number by 1, 3, 5, 7, 9 ... each time beause that is how the difference between perfect squares propogates an finally check if the num is 0.

##### Approach Two

```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        if num == 1:
            return True
        if num == 2:
            return False
        left, right = 2, num
        while left <= right:
            mid = (left + right) // 2
            sqrd = mid * mid
            if sqrd > num:
                right = mid - 1
            elif sqrd < num:
                left = mid + 1
            else:
                return True
        return False
```

We reduce mid by one or increase it by 1 because we want to eliminate numbers that exceed 

For finding square root(not necessarily perfect), use the following approach:

```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        left,right = 1.0, num
        if num < 1.0:
            left,right = num, 1.0 
        while not isclose(left,right):
            mid = (left + right) * 0.5
            sqrd = mid * mid
            if sqrd > num:
                right = mid
            elif sqrd < num:
                left = mid 
        return left
```
