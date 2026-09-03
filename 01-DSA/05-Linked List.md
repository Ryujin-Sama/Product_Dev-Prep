
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

## [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

Sol - Use fast and slow pointer to reach the mid, then reverse the second half of the linked list then assign fast to head and check if fast.val == slow.val if false in anyone return false or else return true

Code Below ->

```
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode slow = head, fast = head, prev, temp;
        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
        }

        prev = slow;
        slow = slow.next;
        prev.next = null;
        while(slow != null){
            temp = slow.next;
            slow.next = prev;
            prev = slow;
            slow = temp;
        }

        fast = head;
        slow = prev;
        while(slow != null){
            if(fast.val != slow.val) return false;
            fast = fast.next;
            slow = slow.next;
        }
        return true;
    }
}
```

Time - O(n)   Space - O(1)


## [328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)

Sol - We keep 3 pointers, 2 pointers to head and head.next and evenhead to keep note of even.
run a loop till even or even.next reached null and keep iterating the and assigning the odd and even accordingly 

Code Below ->

```
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if(head == null || head.next == null) return head;
        ListNode odd = head, even = head.next, evenHead = even;
        while(even != null && even.next != null){
            odd.next = even.next;
            odd = odd.next;
            even.next = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return head;
    }
}

```

Time - O(n)  Space - O(1)


## [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

Sol - we run take pointers fast and slow, for fast we run a loop till n and check if fast == null if yes then we return head.next else we run a loop till fast.next != null  and move slow = slow.next till fast.next reaches null, and once it reached we simply first check if slow.next != null if it's true then update slow.next = slow.next.next and return head;

Code Below ->

```
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode slow = head, fast = head;
        if(head == null) return null;
        int i = 1;
        while(i <= n){
            fast = fast.next;
            i++;
        }
        if(fast == null) return head.next;
        while(fast.next != null){
            fast = fast.next;
            slow = slow.next;
        }
        if(slow.next != null) slow.next = slow.next.next;
        return head;
    }
}
```

Time - O(n)   Space - O(1)


## [2095. Delete the Middle Node of a Linked List](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/)

Sol - Take two pointers slow = head and fast = head.next.next and move the fast until it reaches null then the slow will be reach before the mid pointer we can simply point the small to the next.next of it

Code Below ->

```
class Solution {
    public ListNode deleteMiddle(ListNode head) {
        if(head == null || head.next == null) return null;
        ListNode slow = head;
        ListNode fast = head.next.next;

        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        slow.next = slow.next.next;
        return head;
    }
}
```

Time - O(n)   Space - O(1)