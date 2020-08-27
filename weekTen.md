## Practice Questions of Companies

Round 3

[K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

```python
class Solution:
    def __init__(self):
        #To store the lower side of numbers, the highest one is at the top
        self.max_heap = [] 
    
    def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:   
        for [x,y] in points:
            dist = (x**2) + (y**2)
            pair = (dist, [x,y])
            heappush(self.max_heap, pair)
        vals = heapq.nsmallest(K, self.max_heap)
        
        final = []
        for k,v in vals:
            final.append(v)
        return final
  ```
  
  Solved using heaps
  
  
  [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/submissions/)
  ```python
  class Solution:
    def longestPalindrome(self, s: str) -> str:
        self.l = ""
        
        def expandFromMiddle(s, left, right):
            counter = 0
            while (left >= 0 and right < len(s) and s[left] == s[right]):
              val = s[left:right+1]
              if len(val) > len(self.l):
                    self.l = val
              left -= 1
              right +=1
        
        for i in range(len(s)):
            expandFromMiddle(s, i, i)
            expandFromMiddle(s, i, i + 1)
        return self.l
```
  
  
