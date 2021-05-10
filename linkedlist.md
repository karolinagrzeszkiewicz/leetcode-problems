# Linked Lists

Defining a linked list

```python
class Node(object):
    def __init__(self, d, n = None):
        self.data = d
        self.next_node = n
    def get_next(self):
        return self.next_node
    def set_next(self, n):
        self.next_node = n
    def get_data(self):
        return self.data
    def set_data(self, d):
        self.data = d
class LinkedList(object):
    def __init__(self, r = None):
        self.root = r
        self.size = 0
    def get_size(self):
        return self.size
    def add(self, d):
        new_node = Node(d, self.root)
        self.root = new_node
        self.size += 1
    def remove(self, d):
        this_node = self.root
        prev_node = None #we want to change the pointer of the previous node by connecting it to the next node 
        #to remove this node
        while this_node:
            if this_node.get_data() == d:
                if prev_node:
                    prev_node.set_next(this_node.get_next())
                else:
                    self.root = this.node
                self.size -= 1
                return True
            else:
                prev_node = this_node
                this_node = this_node.get_next()
        return False
    def find(self, d):
        this_node = self.root()
        while this_node:
            if this_node.get_data() == d:
                return d
            else:
                this_node = this_node.get_next()
        return None
        
```

[Reverse a Singly Linked List](https://leetcode.com/problems/reverse-linked-list/)

Reverse a singly linked list.

Example:

Input: 1->2->3->4->5->NULL

Output: 5->4->3->2->1->NULL

Singly Linked List

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

Solution 1 (Iterative)

```python
def reverseList(head):
    prev_head = None #stores the last head accessed
    while head:  #while head != None i.e.until we reach the tail
        curr_head = head
        head = head.next #go to the next element = make it the head
        curr_head.next = prev_head #prepend the current head to the previous head(s)
        prev_head = curr_head #now that we already accessed this head it is stored as the last head accessed
    return prev_head
```

Explanation:

We traverse the input linked list from head to tail by setting consecutive elements as heads (deleting consecutive heads), and pepending these heads to a new linked list from tail to head.

i.e. on each iteration we access the head of the trimmed list and prepend it to the reversed linked list of the heads we already traversed.

Solution 2 (Recursive)

```python
def reverseList(self, head: ListNode) -> ListNode:
        def reverse(curr, prev):
            if not curr:
                return prev #when we reach the end of the list return the last element
            nxt = curr.next #set nxt to next head in input list
            curr.next = prev #set to elements already traversed 
            # for first iteration this gives us head -> none
            prev = curr #set prev to the last head accessed 
            #stores the last head accessed
            curr = nxt #set current head to next head
            return(reverse(curr, prev)) #inductuve step: the reverse list is the reverse tail with the head appended at the end
        #we traverse the list until the tail and then start returning  
        head = reverse(curr = head, prev = None)
        return head
```

Explanation:

base case: when the head is none or when we reach the end of the linked list we return the last element accessed

inductive step: to reverse a linked list we reverse its tail and append its head at the end

[Intersection of Two Linked Lists]()

Write a program to find the node at which the intersection of two singly linked lists begins.

For example, the following two linked lists:

Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3

Output: Reference of the node with value = 8

Input Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.

Singly linked list
```python
class ListNode:
     def __init__(self, x):
        self.val = x
        self.next = None
```

```python
def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        if not headA or not headB:
            return None
 
        pA = headA
        pB = headB
        
        while pA is not pB:
            if pA:
                pA = pA.next
            else:
                pA = headB
            if pB:
                pB = pB.next
            else:
                pB = headA
        
        return pA
```

Explanation:

We use two pointers: A and B. Pointer A traverses linked list A first and when it's complete it starts traversing list B, while pointer B traverses linked list B first and once this is done it traverses linked list A. In this manner the pointers are going to meet at the first element contained by both lists and return the reference to that element i.e. the head of the linked list that follows.

This works because the difference between the number of steps needed to find the common element in list A and lost B is equal to the difference between the number of elements of A and of B (since the elements following the common element are the same and so their length is the same). Thus we can make up for this difference by traversing A and sublist B and B and sublist A: length(A) + length(sublist B) = length(B) + sublist(A)

[Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.

```python
def hasCycle(head):
    try:
        pointer1 = head
        pointer2 = head.next
        while pointer1 is not pointer2:
            pointer1 = pointer1.next
            pointer2 = pointer2.next.next
        return True
    except:
        return False
```