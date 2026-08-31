
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


## [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

Sol - We do it in a two pointer way, one slow and one fast 

Code Below ->

```
class Solution {
    public ListNode middleNode(ListNode head) {
        if(head == null || head.next == null) return head;
        ListNode temp1 = head;
        ListNode temp2 = head;
        while(temp2 != null && temp2.next != null){
            temp1 = temp1.next;
            temp2 = temp2.next.next;
        }
        return temp1;
    }
}
```

Time - O(n)  Space - O(1)

## [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

Sol - We follow a condition that we take total 3 variables, one pointing to null, one pointing to head and one inside the loop to hold the next reference and return the first variable which was pointing to null.

Code Below ->

```
class Solution { 
	public ListNode reverseList(ListNode head) { 
		ListNode prev = null; 
		ListNode curr = head; 
		while (curr != null) { 
			ListNode nextNode = curr.next; // Save next node 
			curr.next = prev; // Reverse direction 
			prev = curr; // Move prev step forward 
			curr = nextNode; // Move curr step forward 
		} 
		return prev; // prev is now the new head
	}
}
```

Time - O(n)   Space - O(1)

