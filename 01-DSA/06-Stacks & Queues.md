
## [225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)

Sol - Use queue and use q.poll() to insert an element int to the queue with the add call 

Code Below ->

```
class MyStack {

    private Queue<Integer> q;

    public MyStack() {

        q = new LinkedList<>();
    }

    public void push(int x) {

        q.add(x);
        for(int i = 0; i < q.size()-1; i++) q.add(q.poll());
    }

    public int pop() {

        return q.poll();
    }

    public int top() {

        return q.peek();
    }

    public boolean empty() {

        return q.isEmpty();
    }
}
/**

 * Your MyStack object will be instantiated and called as such:

 * MyStack obj = new MyStack();

 * obj.push(x);

 * int param_2 = obj.pop();

 * int param_3 = obj.top();

 * boolean param_4 = obj.empty();

 */
```

Time - push - O(n), pop - O(1), top - O(1)    Space - O(Dynamic)


## [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)

Sol - we need to use two stacks, and how we will use is first we add the push into stack 1 and while pop we use a peek function to insert all the element from stack 1 to stack 2 and then pop from stack 2, this way we can maintain the queue function on stack

Code Below ->
```
class MyQueue {

    Stack<Integer> in = new Stack();
    Stack<Integer> out = new Stack();

    public MyQueue() {

    }

    public void push(int x) {

        in.push(x);
    }

    public int pop() {

        peek();
        return out.pop();
    }

    public int peek() {

        if(out.empty()) while(!in.empty()) out.push(in.pop());
        return out.peek();
    }

    public boolean empty() {

        return in.empty() && out.empty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();

 */
```

Time - Push - O(1), pop - O(n) Space - O(2n)


## [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

Sol - Use stack, we insert only the opening braces and check if we get a closing one, we pop and check if it's matching with the top of the stack and in the end if the stack is empty we return true

Code Below ->

```
class Solution {
    public boolean isValid(String s) {
        Stack<Character> st = new Stack<>();
        for(char ch : s.toCharArray()){
            if(ch == '{' || ch == '[' || ch == '(') st.push(ch);
            else{

                if(st.isEmpty()) return false;
                char top = st.pop();
                if(ch == ')' && top != '(') return false;
                else if(ch == ']' && top != '[') return false;
                else if(ch == '}' && top != '{') return false;
            }  
        }
        return st.isEmpty();    
    }
}
```

Time - O(n)   Space - O(n)
