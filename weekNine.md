## More questions

- [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/submissions/)

```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, val=0, left=None, right=None):
    #         self.val = val
    #         self.left = left
    #         self.right = right
    class Solution:

        def height(self, root):
            if not root:
                return 0
            return max(self.height(root.left), self.height(root.right)) + 1

        def isBalanced(self, root: TreeNode) -> bool:
            if not root:
                return True
            lh = self.height(root.left)
            rh = self.height(root.right)

            return abs(lh - rh) <= 1 and self.isBalanced(root.left) and self.isBalanced(root.right)
```

-[Slit a String in Balanced Strings](https://leetcode.com/problems/split-a-string-in-balanced-strings/submissions/)
```python
class Solution:
    def balancedStringSplit(self, s: str) -> int:
        no_strings = 0
        count = 0
        for char in s:
            if char == "R":
                count += 1
            else:
                count -= 1
            if count == 0:
                no_strings += 1
        return no_strings
```

- [Find Common Characters](https://leetcode.com/problems/find-common-characters/)
```python
class Solution:
    def commonChars(self, A: List[str]) -> List[str]:
        if not A:
            return []
        ideal = dict(collections.Counter(A[0]))
        for i in range(1, len(A)):
            word = collections.Counter(A[i]) 
            keys = list(ideal.keys())
            for k in keys:
                if k not in word:
                    del ideal[k]
                else:
                    ideal[k] = min(ideal[k], word[k])
        
        final = []
        for k in ideal.keys():
            for i in range(ideal[k]):
                final.append(k)
        return final      
```
