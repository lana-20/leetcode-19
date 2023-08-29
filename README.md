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

## Approach 1: Two-Pass Algo

### Intuition

We notice that the problem could be simply reduced to another one : Remove the (L - n + 1)th node from the beginning in the list, where L is the list length. This problem is easy to solve once we found list length L.

### Algorithm

First we will add an auxiliary "dummy" node, which points to the list head. The "dummy" node is used to simplify some corner cases such as a list with only one node, or removing the head of the list. On the first pass, we find the list length L. Then we set a pointer to the dummy node and start to move it through the list till it comes to the (L − n) th node. We relink next pointer of the (L − n)th node to the (L - n + 2)th node and we are done.
![image](https://github.com/lana-20/leetcode-19/assets/70295997/11bfaa41-a4f2-4cbd-8598-48572084c04b)


Java Solution Code:

```
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    int length  = 0;
    ListNode first = head;
    while (first != null) {
        length++;
        first = first.next;
    }
    length -= n;
    first = dummy;
    while (length > 0) {
        length--;
        first = first.next;
    }
    first.next = first.next.next;
    return dummy.next;
}
```

Complexity Analysis:

- Time complexity : O(L).

- The algorithm makes two traversal of the list, first to calculate list length L and second to find the (L - n) th node. There are 2L-n operations and time complexity is O(L).

- Space complexity : O(1).

- We only used constant extra space.


## Approach 2: One-Pass Algo

### Algorithm

The above algorithm could be optimized to one pass. Instead of one pointer, we could use two pointers. The first pointer advances the list by n+1 steps from the beginning, while the second pointer starts from the beginning of the list. Now, both pointers are exactly separated by n  nodes apart. We maintain this constant gap by advancing both pointers together until the first pointer arrives past the last node. The second pointer will be pointing at the nnnth node counting from the last.
We relink the next pointer of the node referenced by the second pointer to point to the node's next next node.

[image/drawing TBD]

Java Solution Code:

```
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode first = dummy;
    ListNode second = dummy;
    // Advances first pointer so that the gap between first and second is n nodes apart
    for (int i = 1; i <= n + 1; i++) {
        first = first.next;
    }
    // Move first to the end, maintaining the gap
    while (first != null) {
        first = first.next;
        second = second.next;
    }
    second.next = second.next.next;
    return dummy.next;
}
```

Complexity Analysis:

- Time complexity : O(L).

- The algorithm makes one traversal of the list of L nodes. Therefore time complexity is O(L).

- Space complexity : O(1).

- We only used constant extra space.

----

Python Solution Code:
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

Python Code with Inline Comments:
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


