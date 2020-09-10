## Hacker Ranks Solved


[Balanced Sum](https://www.hackerrank.com/contests/world-codesprint-11/challenges/balanced-array)

```python
def balancedSum(arr): right_sum, left_sum = 0, 0 size = len(arr)
  for i in range(1, size):
    right_sum += arr[i] 
  i, j = 0, 1
  while j < size :
    right_sum -= arr[j] left_sum += arr[i]
    if left_sum == right_sum:
      return i + 1 
    j += 1
    i += 1 
 return -1
 ```
 
 [Number of groups- Similar question](https://www.hackerrank.com/challenges/connected-cell-in-a-grid/problem)
 
 ```python
import collections 

def numGroups(grid):
  if not grid: 
    return 0
  row = len(grid) 
  no_groups = 0 
  visited = set()

  def bfs(ro):
    stack = collections.deque() stack.append(ro)
    while stack:
      row_pop = stack.pop() row_array = grid[row_pop]
      for r in range(len(row_array)):
        if (row_array[r] == "1" and r not in visited): visited.add(r)
          stack.append(r)
          
  for ro in range(row):
    if ro not in visited:
      bfs(ro)
      no_groups += 1 
    return no_groups
    
def countGroups(related): 
  arr = []
  for string in related: 
    int_arr = []
    for char in string: 
      int_arr.append(char)
    arr.append(int_arr) 
 return numGroups(arr)
 
 ```
