
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

## [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

Sol - Use slow and fast pointer, if they meet return true, else false

Code Below ->

```
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        if(head == null || head.next == null) return false;
        while(fast.next != null && fast.next.next != null){
            slow = slow.next;
            fast = fast.next.next;
            if(slow == fast) return true;
        }
        return false;
    }
}
```

Time - O(n)  Space - O(1)


## [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

Sol - we solve this problem by 2 steps - first detect the loop using fast and slow pointers then we assign the slow as head and move both at one pointer each and once they collide we return slow
why it will collide bcoz - while moving slow to the point of loop cycle take it as L1, and fast pointer would have moved 2L1, and it might need d points to cover to reach slow pointer and on every move the d reduces to d-1 pointer since fast moved 2 times and slow moved 1 time and when they colide it then the total distance by slow pointer is L1 + d and by fast pointer is 2(l1 + d)
the length of the cycle is $$2(L_1 + d) - (L_1 + d) = k \cdot C$$
where k  = no of turn fast took and if k == 1 then L1 + d = C so now to meet the starting point then L1 = C  -  d, so we move one pointer each of slow and fast until they meet again.

Code Below ->

```
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head == null || head.next == null) return null;
        ListNode slow = head;
        ListNode fast = head;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
            if(slow == fast) break;
        }
        if(fast == null || fast.next == null) return null;
        slow = head;
        while(slow != fast){
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
}
```

Time - O(n)   Space - O(1)

