
## [237. Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/)

Sol - We just set the current val to the next one and same with the address for it.

Code Below ->

```
/**
 * Definition for singly-linked list.
* public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 /
class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
```

Time - O(1)    Space - O(1)