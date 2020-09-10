## Hacker Ranks Solved


### Scenario 1


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
 
 
 ### Scenario 2
 
 [Robot Bounded in Circle](https://leetcode.com/problems/robot-bounded-in-circle/)
 
 ```python
 class Solution:
    def isRobotBounded(self, instructions: str) -> bool:
        
        ## directions are right, bottom, left, top
        direction = [[0,1], [-1,0], [0,-1], [1,0]]
        point = [0,0]
        
        ## Index representing the direction we want to access
        direction_idx = 0

        for ins in instructions:
            if ins == "G":
                point = [sum(x) for x in zip(point, direction[direction_idx])]
            elif ins == "L":
                direction_idx = (direction_idx + 1) % 4
            elif ins == "R":
                direction_idx = (direction_idx - 1) % 4

        if (point != [0,0] and direction_idx == 0):
                return False

        ## For robot to be unbounded, point has to change and direction should not be 0 in one iteration of the sequence of events     
        return True
```


[Paint Master](https://www.hackerrank.com/contests/spider-preinduction/challenges/paint-master)

```
import collections
def strokesRequired(picture):
    if not picture:
        return 0
    row = len(picture)
    column = len(picture[0])
    no_of_strokes = 0
    visited = set()

    def bfs(ro, col, val):
        stack = collections.deque()
        stack.append((ro, col))
        while stack:
            row_pop, col_pop = stack.pop()
            directions = [[1, 0], [-1,0], [0,1], [0,-1]]
            for r1, c1 in directions:
                r = r1 + row_pop
                c = c1 + col_pop
                if (r in range(row)) and (c in range(column)) and picture[r][c] == val and (r,c) not in visited:
                    visited.add((r, c))
                    stack.append((r,c))

    for ro in range(row):
        for col in range(column):
            val = picture[ro][col]
            if (ro, col) not in visited:
                bfs(ro, col,val)
                no_of_strokes += 1
    return no_of_strokes
```
