# Leetcode 19: Remove Nth Node from End of List

Given the head of a linked list, remove the nth node from the end of the list and return its head.

 

__Example 1:__

![image](https://github.com/lana-20/leetcode-19/assets/70295997/afcc44c7-18a6-45a9-88c9-ec8229f3350e)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

__Example 2:__
```
Input: head = [1], n = 1
Output: []
```

__Example 3:__
```
Input: head = [1,2], n = 1
Output: [1]
```

__Constraints:__
```
The number of nodes in the list is sz.
1 <= sz <= 30
0 <= Node.val <= 100
1 <= n <= sz
```

Solution Code:
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode(0, head)
        left = dummy
        right = head

        while n > 0 and right:
            right = right.next
            n -= 1

        while right:
            left = left.next
            right = right.next

        # delete
        left.next = left.next.next
        return dummy.next
```

Code with Inline Comments:
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        # Create a dummy node. We don't care what the value of this node is, let's say 0. And we want to make sure that the next pointer of this node is set to the head of the list, because we are inserting it at the beginning.
        dummy = ListNode(0, head)
        # Initialize the 'left' pointer to dummy.
        left = dummy
        # We want right pointer to be head + n. We need a loop to do that. We can't just do that with a calculation.
        # We can initially say right=head ...
        right = head

        # ... And while n is greater than 0 and 'right' is not Null, we want to keep shifting right.
        while n > 0 and right:
            right = right.next
            # Decrement n by 1. Because once n=0, it means we've shifted by the desirable amount.
            n -= 1

        # Keep shifting both pointers 'left' and 'right'. Keep going until 'right' reaches the end of the list.
        while right:
            left = left.next
            right = right.next

        # Delete the node. Update the 'left' node's next pointer - shift it by one.
        left.next = left.next.next
        # dummy.next is at the head of the list. Return the updated list. We don't want to include the dummy node in the output. We never wanted to add a node to the list, we just want to remove a node.
        return dummy.next

```


