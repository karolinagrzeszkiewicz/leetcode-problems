# Stacks

[MinStack](https://leetcode.com/problems/min-stack//)

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

push(x) -- Push element x onto stack.

pop() -- Removes the element on top of the stack.

top() -- Get the top element.

getMin() -- Retrieve the minimum element in the stack.

```python
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = [] #stack is a kind of list: here in order to get O(1) complexity of retrieving the minimum element,
        # we implement a stack consisting of tuples (a, min) where a is the element and a is the minimum element of the elements
        #already in the stack

    def push(self, x: int) -> None:
        if self.stack:
            self.stack.append((x, min(x, self.getMin()))) #next tuple consits of next element and the minimum of that
            #element and the minimum of the stack - given that the first minimum is the bottom element the minimum keeps
            #decreasing
        else:
            self.stack.append((x,x)) #first element and the mimum so far = first element

    def pop(self) -> None:
        if self.stack:
            return self.stack.pop() #remove the top element i.e. last element of the list
        else:
            None

    def top(self) -> int:
        if self.stack:
            return self.stack[-1][0] #get the last element of the list
        else:
            None
        

    def getMin(self) -> int:
        if self.stack:
            return self.stack[-1][1] #get the last minimum 
        else:
            None
```

