
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


## [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)

Sol - Using hashing, store the freq of the ch on one iteration and subtract the count in the second iteration, if anagram then the freq should be zero on every ch, if false then return false.

Code Below ->

```
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length()) return false;
        int[] freq  = new int[26];
        for(int i = 0; i < s.length(); i++){
            freq[s.charAt(i) - 'a']++;
        }
        for(int i = 0; i < t.length(); i++){
            freq[t.charAt(i) - 'a']--;
        }
        for(int i = 0; i < 26; i++){
            if(freq[i] != 0) return false;
        }
        return true;
    }
}
```

Time - O(n)   Space - O(26)

## [1903. Largest Odd Number in String](https://leetcode.com/problems/largest-odd-number-in-string/)

Sol - Start from the back since if need to only check the unit place to find if it's odd or even so we check from back and if it's odd then return substring(o , i + 1);

Code  Below ->

```
class Solution {
    public String largestOddNumber(String num) {
        int ind = -1;
        int i;
        for(i = num.length()-1; i >= 0; i--){
            if((num.charAt(i) - '0') % 2 != 0){
                return num.substring(0, i + 1);
            }
        }
        return "";
    }
}
```

Time - O(n)    Space - O(1)

## [14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

Sol - First we sort the string array and take the two string the first one and the last one, and traverse through two of them with i <= Math.min(first.length(), last.length()),
the moment we find first.charAt(i) != last.charAt(i) we return the ans, or else we keep on appending the ans.

Code Below ->

```
class Solution {
    public String longestCommonPrefix(String[] strs) {
        StringBuilder ans = new StringBuilder();
        Arrays.sort(strs);
        String first = strs[0];
        String last = strs[strs.length - 1];
        for(int i = 0; i <= Math.min(first.length(), last.length()); i++){
            if(first.charAt(i) != last.charAt(i)) return ans.toString();
            ans.append(first.charAt(i));
        }
        return"";
    }
}
```

Time - O(n)   Space - O(1)