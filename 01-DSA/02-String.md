
## [1021. Remove Outermost Parentheses](https://leetcode.com/problems/remove-outermost-parentheses/)

Sol - We gonna use a String-builder and iterate through the string with a counter if we find '(' then increment and if we find ')' then decrement and and before increment if the cnt is +ve then append to the new string '(' and  decrement first and check if counter is +ve > 0 then append ')' to the new string and return.

Code below -> 

```
class Solution {
    public String removeOuterParentheses(String s) {
        StringBuilder ans = new StringBuilder();
        int a = 0;
        for(char ch : s.toCharArray()){
            if(ch == '('){
                if(a > 0) ans.append(ch);
                a++;
            }
            else if(ch == ')'){
                a--;
                if(a > 0) ans.append(ch);
            }
        }
        return ans.toString();
    }
}
```

Time - 0(n)  Space - 0(1)
