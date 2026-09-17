
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


## [155. Min Stack](https://leetcode.com/problems/min-stack/)

Sol - We follow certain condition, first we check if the stack isEmpty if yes then assign the min with the value and if stack not empty and val > min then we push the value and if val < min then we do an operation 2 x valu - min and insert this value into the stack and update min with this value.
for pop operation as we follow the same , same with top function as well

Code Below ->

```
class MinStack {

    Stack<Long> st = new Stack<>();
    long min;

    public MinStack() {

    }

    public void push(int value) {

        if(st.isEmpty()){
            min = (long)value;
            st.push((long)value);
        }

        else{
            if((long)value > min) st.push((long)value);
            else{
                st.push(2L * (long)value - min);
                min = (long)value;
            }
        }
    }

    public void pop() {

        long x = st.pop();
        if(x < min) min = 2 * min - x;
    }

    public int top() {

        long x = st.peek();
        return x < min ? (int)min : (int)x;
    }

    public int getMin() {
        return (int)min;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(value);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

Time - Push - O(1), pop - O(1)   Space - O(N)

## Infix to Postfix Conversion

- Start by scanning the infix expression from left to right.
- If the scanned character is an operand, print it immediately.
- If the scanned character is an operator:

- If the precedence of the operator is greater than the operator in the stack, or the stack is empty, or the stack contains a ‘(’, push the operator into the stack.
- Otherwise, pop all operators from the stack with higher or equal precedence than the scanned operator, then push the scanned operator into the stack.

- If the scanned character is a ‘(’, push it into the stack.
- If the scanned character is a ‘)’, pop the stack and output the operators until a ‘(’ is encountered, and discard both parentheses.
- Repeat steps 2-5 until the entire infix expression has been scanned.
- Print the output.
- Finally, pop and print all remaining operators in the stack until it is empty

Code Below ->

```
class Solution {

    // Function to return precedence of operators
    public static int prec(char c) {
        if (c == '^')  // Exponent operator has highest precedence
            return 3;
        else if (c == '/' || c == '*')  // Multiplication and division have higher precedence than addition
            return 2;
        else if (c == '+' || c == '-')  // Addition and subtraction have lowest precedence
            return 1;
        else
            return -1;
    }

    // Function to convert infix expression to postfix expression
    public static void infixToPostfix(String s) {
        Stack<Character> st = new Stack<>();  // Stack to hold operators and parentheses
        StringBuilder result = new StringBuilder();  // StringBuilder to hold the resulting postfix expression

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);

            // If the scanned character is an operand, add it to the result string
            if (Character.isLetterOrDigit(c)) {
                result.append(c);
            }

            // If the scanned character is an ‘(‘, push it to the stack
            else if (c == '(') {
                st.push('(');
            }

            // If the scanned character is a ‘)’, pop from stack until an ‘(‘ is encountered
            else if (c == ')') {
                while (st.peek() != '(') {
                    result.append(st.pop());
                }
                st.pop();  // Pop the ‘(‘ from the stack
            }

            // If an operator is scanned
            else {
                while (!st.isEmpty() && prec(c) <= prec(st.peek())) {
                    result.append(st.pop());
                }
                st.push(c);  // Push the current operator to the stack
            }
        }

        // Pop all the remaining elements from the stack
        while (!st.isEmpty()) {
            result.append(st.pop());
        }

        System.out.println("Postfix expression: " + result.toString());  // Output the result
    }
}

public class Main {

    public static void main(String[] args) {
        String exp = "(p+q)*(m-n)";  // Infix expression
        System.out.println("Infix expression: " + exp);
        Solution.infixToPostfix(exp);  // Convert the infix expression to postfix
    }
}
```

Time - O(N)   Space - O(N)


## Infix to Prefix Conversion

- Reverse the given infix expression.
- Scan the expression from left to right.
- When an operand is encountered, print it directly.
- If the operator is encountered and the stack is empty, push the operator into the stack.
- If the incoming operator has higher precedence than the top of the stack, push it into the stack.
- If the incoming operator has the same precedence as the top of the stack, push it into the stack.
- If the incoming operator has lower precedence than the top of the stack, pop and print the top of the stack. Then, test the incoming operator against the top of the stack again and continue popping operators from the stack until it finds an operator with lower or the same precedence.
- If the incoming operator has the same precedence as the top of the stack and the incoming operator is '^', pop the top of the stack until the condition is met. If not, push the '^' operator.
- If the operator is ')', push it into the stack.
- If the operator is '(', pop all operators from the stack until a ')' is encountered.
- If the top of the stack is ')', push the operator onto the stack.
- At the end of the expression, pop and print all remaining operators from the stack.
- Finally, reverse the output and print it as the prefix expression.

Code Below ->

```
class Solution {

    // Function to return precedence of operators
    public static int getPriority(char C) {
        if (C == '^')  // Exponent operator has highest precedence
            return 3;
        else if (C == '*' || C == '/')  // Multiplication and division have higher precedence than addition
            return 2;
        else if (C == '+' || C == '-')  // Addition and subtraction have lowest precedence
            return 1;
        return 0;
    }

    // Function to convert infix expression to postfix expression
    public static String infixToPostfix(String infix) {
        infix = '(' + infix + ')';  // Add parentheses to handle edge cases
        int l = infix.length();
        Stack<Character> stack = new Stack<>();  // Stack to store operators
        StringBuilder result = new StringBuilder();  // StringBuilder to store the resulting postfix expression

        for (int i = 0; i < l; i++) {
            char c = infix.charAt(i);

            // If the scanned character is an operand, add it to output
            if (Character.isLetterOrDigit(c))
                result.append(c);

            // If the scanned character is ‘(’, push it to the stack
            else if (c == '(')
                stack.push('(');

            // If the scanned character is ‘)’, pop and output from the stack until an ‘(‘ is encountered
            else if (c == ')') {
                while (stack.peek() != '(') {
                    result.append(stack.pop());
                }
                stack.pop();  // Remove '(' from the stack
            }

            // If an operator is found
            else {
                while (!stack.isEmpty() && getPriority(c) <= getPriority(stack.peek())) {
                    result.append(stack.pop());
                }
                stack.push(c);  // Push current operator on stack
            }
        }

        // Pop all remaining elements from the stack
        while (!stack.isEmpty()) {
            result.append(stack.pop());
        }
        return result.toString();  // Return the postfix expression
    }

    // Function to convert infix expression to prefix expression
    public static String infixToPrefix(String infix) {
        StringBuilder sb = new StringBuilder(infix);

        // Reverse the infix expression
        sb.reverse();

        // Replace '(' with ')' and vice versa
        for (int i = 0; i < sb.length(); i++) {
            if (sb.charAt(i) == '(')
                sb.setCharAt(i, ')');
            else if (sb.charAt(i) == ')')
                sb.setCharAt(i, '(');
        }

        // Get the postfix of the modified string
        String prefix = infixToPostfix(sb.toString());

        // Reverse the postfix to get the prefix
        return new StringBuilder(prefix).reverse().toString();  // Return the prefix expression
    }
}

public class Main {

    public static void main(String[] args) {
        String exp = "(p+q)*(c-d)";  // Infix expression
        System.out.println("Infix expression: " + exp);
        System.out.println("Prefix Expression: " + Solution.infixToPrefix(exp));  // Output the prefix expression
    }
}
```

Time - O(n)    Space - O(n)


## Postfix to Infix Conversion

Sol - 
- Traverse the postfix expression from left to right.
- Use a stack to store operands.
- For each operator, pop two operands, combine them in infix order with parentheses, and push the result back.
- The final item in the stack will be the infix expression.

Code Below ->

```
import java.util.*;

class PostfixToInfix {
    // Function to convert postfix to infix
    public String postfixToInfix(String postfix) {
        Stack<String> s = new Stack<>();
        int n = postfix.length();

        // Traverse the postfix expression from left to right
        for (int i = 0; i < n; i++) {
            char c = postfix.charAt(i);

            // If the character is an operand, push it to the stack
            if (Character.isLetterOrDigit(c)) {
                s.push(String.valueOf(c));
            } else {
                // Pop two operands from the stack
                String op2 = s.pop();
                String op1 = s.pop();

                // Form the new infix expression and push back to stack
                s.push("(" + op1 + c + op2 + ")");
            }
        }

        // The final element in the stack is the result
        return s.peek();
    }
}

public class Main {
    public static void main(String[] args) {
        PostfixToInfix converter = new PostfixToInfix();
        String postfix = "AB*C+";
        System.out.println("Infix Expression: " + converter.postfixToInfix(postfix));
    }
}

```

Time - O(n)   Space - O(n)


## Prefix to Infix Conversion

Sol - 
- Traverse the prefix expression from right to left.
- Use a stack to store operands.
- For each operator, pop two operands from the stack, wrap them in parentheses, and push the resulting expression back.
- The final item in the stack will be the infix expression.

Code Below ->

```
import java.util.*;

class PrefixToInfix {
    // Function to convert prefix to infix
    public String prefixToInfix(String prefix) {
        Stack<String> s = new Stack<>();
        int n = prefix.length();

        // Traverse the prefix expression from right to left
        for (int i = n - 1; i >= 0; i--) {
            char c = prefix.charAt(i);

            // If the character is an operand, push it to the stack
            if (Character.isLetterOrDigit(c)) {
                s.push(String.valueOf(c));
            } else {
                // Pop two operands from the stack
                String op1 = s.pop();
                String op2 = s.pop();

                // Form the new infix expression and push back to stack
                s.push("(" + op1 + c + op2 + ")");
            }
        }

        // The final element in the stack is the result
        return s.peek();
    }
}

public class Main {
    public static void main(String[] args) {
        PrefixToInfix converter = new PrefixToInfix();
        String prefix = "*-A/BC-/AKL";
        System.out.println("Infix Expression: " + converter.prefixToInfix(prefix));
    }
}

```

Time - O(N)   Space - O(N)


## Postfix to Prefix Conversion

Sol - 
- Traverse the postfix expression from left to right.
- Use a stack to store operands.
- For each operator, pop two operands, combine them with the operator in prefix order, and push the result back.
- The final item in the stack will be the prefix expression.

Code Below ->

```
import java.util.*;

class PostfixToPrefix {
    // Function to convert postfix to prefix
    public String postfixToPrefix(String postfix) {
        Stack<String> s = new Stack<>();
        int n = postfix.length();

        // Traverse the postfix expression from left to right
        for (int i = 0; i < n; i++) {
            char c = postfix.charAt(i);

            // If the character is an operand, push it to the stack
            if (Character.isLetterOrDigit(c)) {
                s.push(String.valueOf(c));
            } else {
                // Pop two operands from the stack
                String op2 = s.pop();
                String op1 = s.pop();

                // Form the new prefix expression and push back to stack
                s.push(c + op1 + op2);
            }
        }

        // The final element in the stack is the result
        return s.peek();
    }
}

public class Main {
    public static void main(String[] args) {
        PostfixToPrefix converter = new PostfixToPrefix();
        String postfix = "ABC/-AK/L-*";
        System.out.println("Prefix Expression: " + converter.postfixToPrefix(postfix));
    }
}

```

Time - O(n)   Space - O(n)

## Prefix to Postfix Conversion

Sol - 
- Traverse the prefix expression from right to left.
- Use a stack to store operands.
- For each operator, pop two operands from the stack, combine them with the operator, and push the result back.
- The final item in the stack will be the postfix expression.

Code Below ->

```
import java.util.*;

class PrefixToPostfix {
    // Function to convert prefix to postfix
    public String prefixToPostfix(String prefix) {
        Stack<String> s = new Stack<>();
        int n = prefix.length();

        // Traverse the prefix expression from right to left
        for (int i = n - 1; i >= 0; i--) {
            char c = prefix.charAt(i);

            // If the character is an operand, push it to the stack
            if (Character.isLetterOrDigit(c)) {
                s.push(String.valueOf(c));
            } else {
                // Pop two operands from the stack
                String op1 = s.pop();
                String op2 = s.pop();

                // Form the new postfix expression and push back to stack
                s.push(op1 + op2 + c);
            }
        }

        // The final element in the stack is the result
        return s.peek();
    }
}

public class Main {
    public static void main(String[] args) {
        PrefixToPostfix converter = new PrefixToPostfix();
        String prefix = "*-A/BC-/AKL";
        System.out.println("Postfix Expression: " + converter.prefixToPostfix(prefix));
    }
}

```

Time - O(n)   Space - O(n)