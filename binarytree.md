# Binary Trees

Defining a binary tree node

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

[Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

```python
def isSymmetric(root):
        def is_mirror(t1, t2):
            if (t1 == None and t2 == None):
                return True
            elif (t1 == None or t2 == None):
                return False
            else:
                return ((t1.val == t2.val) and is_mirror(t1.right, t2.left) and is_mirror(t1.left, t2.right))
        if root == None:
            return True
        else:
            return (is_mirror(root.right, root.left))
```

Explanation:

Recursive solution with

base case: if two subtrees are None, then they are symmetric. If one of them is None, then they are not symmetric.

inductive step: to check if two subtrees are symmetric, check if the subtrees of each of the two subtrees are symmetric. 

[Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

Invert a binary tree.

```python
def invertTree(root):
        def swap(r):
            if r == None:
                return r  
            else:
                t1 = r.left
                t2 = r.right
                r.right = swap(t1)
                r.left = swap(t2)                
                return r
        return(swap(root))
```

Explanation:

Recursive solution with

Base case: if a node is empty then return the node.

Inductive step: to swap two substrees, first swap the subtrees of each of the two subtrees.

[Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/)

Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not.

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of new tree.

```python
def mergeTrees(t1, t2):
        def merge(t1, t2):
            if t1 == None:
                return t2
            elif t2 == None:
                return t1
            t1.val += t2.val
            t1.left = merge(t1.left, t2.left)
            t1.right = merge(t1. right, t2.right)
            return t1
        return merge(t1, t2)
```

Explanation:
    
Recursive solution with:

Base case: if one of the nodes to be merged is empty, we return the non-empty node.

Inductive step: merging two trees means merging the two left subtrees together and merging the two right subtrees together.

Note: By convention we merge here by adding the values of t2 to the respective values of t1 as we traverse the two trees simulatenously, but equivalently we could add the values of t1 to t2 and return the modified t2. 

Direction of recursion: We go from roots to leaves and start returning once we reach the leaves and go backwards to the roots.

[Path Sum](https://leetcode.com/problems/path-sum/)

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

Note: A leaf is a node with no children.

```python
def hasPathSum(self, root, sum):
    if root == None:
        return False
    elif root.val == sum and root.left == None and root.right == None:
        return True
    cur_val = root.val
    return self.hasPathSum(root.left, sum - cur_val) or self.hasPathSum( root.right, sum - cur_val) 
```

[Longest Univalue Path](https://leetcode.com/problems/longest-univalue-path/)

Given the root of a binary tree, return the length of the longest path, where each node in the path has the same value. This path may or may not pass through the root.

The length of the path between two nodes is represented by the number of edges between them.

```python
def longestUnivaluePath(root):
    longest_arrow = 0
    #tracks the longest arrow so far

    def arrow_length(node):
        if not node:
            return 0
        else:

            #recursive part
            left_length = arrow_length(node.left)
            right_length = arrow_length(node.right)

            #if same value on parent and left/right child extend the arrow
            left_arrow = left_length + 1 if node.left and node.left.val == node.val else 0
            right_arrow = right_length + 1 if node.right and node.right.val == node.val else 0

            nonlocal longest_arrow
            longest_arrow = max(longest_arrow, right_arrow + left_arrow) #for parent node we can take the sum of left 
            #and right arrows


            return max(left_arrow, right_arrow) #when arrow_length is applied to left or right child node 
        #only one of the arrows extending from these children can be chosen

    arrow_length(root)
    return longest_arrow

#based on the idea that parent (top) node of the univalue path can have two univalue children but its children
#can only have one univalue child/arrow
```

Explanation:

Recursive solution with Python's nonlocal variable. The helper function arrow_length is a
recursive function:

base case: 0 when there is no tree or when we reach the end of nodes. Or when node values are different. 

inductive step: when the left univalue branch has some length l and the node has the same value then the merged univalue branch has l+1, same for the righ branch. In such case left univalue branch length is the maximum of its right subbranch and left subbranch. Same for the right univalue branch length. 

However, for the parent of a univalue path (its top node) it is the maximum of the longest path found so far and the sum of the lengths of its left univalue branch and right univalue branch, depending on which is biggest.

We use the longest_arrow nonlocal variable (the return of longestUnivaluePath) and the return of the arrow_length helper function to differentiate between constructing a univalue path for a univalue parent and a univalue child node. 

