
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

## [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/)

Sol - We kind of map between 2 strings and check the incremented value is same for the character and if different we return false

Code Below ->

```
class Solution {
    public boolean isIsomorphic(String s, String t) {
        int[] s1 = new int[256], s2 = new int[256];
        for(int i = 0; i < s.length(); i++){
            if(s1[s.charAt(i)] != s2[t.charAt(i)]) return false;
            s1[s.charAt(i)] = i + 1;
            s2[t.charAt(i)] = i + 1;
        }
        return true;
    }
}
```

Time - O(n)   Space - O(1)

## [796. Rotate String](https://leetcode.com/problems/rotate-string/)

Sol - Concatenate both the string and search in the string.

Code Below ->

```
class Solution {
    public boolean rotateString(String s, String goal) {
        if(s.length() != goal.length()) return false;
        // for(int i = 0; i < s.length(); i++){
        //     String rot = s.substring(i) + s.substring(0, i);
        //     if(rot.equals(goal)) return true;
        // }
        String rot = s + s;
        if(rot.contains(goal)) return true;
        return false;
    }
}
```

Time - O(1)   Space - O(1)


## [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

Sol - There are multiple solution 
1. Using hashmap and priority queue -> we iterate through an array and count the frequency and then implement a priority queue with a lambda function to make it max heap wise so that the max size char stays at top, then we using stringbuilder we pop every element and add it the no of times it has the frequency  TC - (O(n log k))  Space - O(K) k is the no of unique element
2. Using Frequency map array -> we take the frequency array with freq(128) and keep on increasing the count of each letter and then we iterate again on the freq array with max count first and keep on appending it the no of times it appears TC - O(n x k)  Space - O(128)

Code Below -> 
1. 
```
 class Solution {

    public String frequencySort(String s) {

        Map<Character, Integer> freq = new HashMap<>();
        for(char c : s.toCharArray()) freq.put(c, freq.getOrDefault(c,0) + 1);
        PriorityQueue<Map.Entry<Character,Integer>> pq = new PriorityQueue<>( (a, b) -> b.getValue() - a.getValue());

        pq.addAll(freq.entrySet());
        StringBuilder res = new StringBuilder();
        while(!pq.isEmpty()){
            Map.Entry<Character, Integer> entry = pq.poll();
            res.append(String.valueOf(entry.getKey()).repeat(entry.getValue()));
        }
        return res.toString();
    }
}
```

2. 
```
 class Solution {

    public String frequencySort(String s) {
        StringBuilder sb = new StringBuilder();
        int[] freq = new int[128];
        int n = s.length();

        for (int i = 0; i < n; i++) {
            freq[s.charAt(i)]++;
        }

        while (sb.length() < n) {
            int max = 0;

            for (int i = 1; i < 128; i++) {
                if (freq[i] > freq[max]) {
                    max = i;
                }
            }

            for (int i = 0; i < freq[max]; i++) {
                sb.append((char) max);
            }
            freq[max] = 0;
        }
        return sb.toString();
    }
}
```

1.  Time - O(n Log K)   Space - O(k)  k is the no of unique elements
2.  Time - O(n)  Space - O(128)

## [1614. Maximum Nesting Depth of the Parentheses](https://leetcode.com/problems/maximum-nesting-depth-of-the-parentheses/)

Sol - we keep the cnt and ans and on each char each if '(' the cnt++ and if ')' then we do a cnt--
and keep the max count in the ans.

Code Below ->

```
class Solution {
    public int maxDepth(String s) {
        int cnt = 0, ans = 0;
        for(char ch: s.toCharArray()){

            if(ch == '(')cnt++;
            else if(ch == ')') cnt--;
            ans = Math.max(cnt, ans);
        }
        return ans;
    }
}
```

Time - O(n)   Space - O(1)


## [13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

Sol - Keep a map DS and keep the entry of all the roman numerals and traverse through the string with conditions of if the current value is less then the next value then subtract  the numbers and if it's more then the next then keep adding the same.

Code Below ->

```
class Solution {
    public int romanToInt(String s) {
       Map<Character, Integer> mpp = Map.of('I', 1, 'V', 5, 'X', 10, 'L', 50, 'C', 100, 'D', 500, 'M', 1000);
       int res = 0;
       for(int i = 0; i < s.length(); i++){
	        int curr = mpp.get(s.charAt(i));
	        int next = (i + 1 < s.length()) ? mpp.get(s.charAt(i + 1)) : 0;
	        if(curr < next) res -= curr;
	        else res += curr;
       }
        return res;
    }
}
```

Time - O(n)   Space - O(1)

## [8. String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)

Sol - We reach the sol step by step, we trim the string to eliminate empty spaces, then we check the first letter if it's '-' then we keep the sign as 0 else we keep it 1 and increment the i, and we run a while loop till s.length(), and if we find any ch which is > '9' or < '1' then we break or else we keep adding to the res as res * 10 + (ch - '0') and at the end we return with sign *  res

Code below ->

```
class Solution {
    public int myAtoi(String s) {
        s = s.trim();
        int sign = 1, i = 0;
        long res = 0;

        if(s.length() == 0) return 0;
        if(s.charAt(0) == '-') {
            sign = -1; i++;
        }
        else if(s.charAt(0) == '+') i++;

        while(i < s.length()){

            char ch = s.charAt(i);
            if(ch < '0' || ch > '9') break;
            res = res * 10 + (ch - '0');
            if(sign * res > Integer.MAX_VALUE) return Integer.MAX_VALUE;
            if(sign * res < Integer.MIN_VALUE) return Integer.MIN_VALUE;
            i++;
        }
        return (int) (sign * res);
    }
}
```

Time - O(n)    Space - O(1)

## [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

Sol - 

1. **String Transformation:** Insert `#` between characters and add unique sentinels `^` and `$` at the boundaries (e.g., `"babad"` $\rightarrow$ `"^#b#a#b#a#d#$"`). * Unifies odd and even length palindromes into odd-length searches. * Eliminates out-of-bounds checks. 
2. **Reuse Computations via Symmetry:** Maintain the center ($C$) and right boundary ($R$) of the rightmost palindrome found so far. * For index $i < R$, compute mirror position: $\text{mirror} = 2C - i$. * Seed the palindrome radius: `p[i] = Math.min(R - i, p[mirror])`. 
3. **Expand Outward:** Only run character comparisons beyond $R$

Code Below -> 

```
class Solution {
    public String longestPalindrome(String s) {
        if(s == null || s.length() ==0) return "";
        StringBuilder sb = new StringBuilder("^");
        for(int i = 0; i < s.length(); i++){
            sb.append("#").append(s.charAt(i));
        }
        sb.append("#$");
        String t = sb.toString();
        int n = t.length();
        int[] p = new int[n];
        int c = 0, r = 0;
        int maxLen = 0;
        int maxCenter = 0;

        for(int i = 1; i < n - 1; i++){
            int mirror = 2 * c - i;
            if(i < r) p[i] = Math.min(r-i, p[mirror]);
            while(t.charAt(i + 1 + p[i]) == t.charAt(i - 1 - p[i])) p[i]++;
            if(i + p[i] > r){
                c = i;
                r = i + p[i];
            }
            if(p[i] > maxLen){
                maxLen = p[i];
                maxCenter = i;
            }
        }
        int st = (maxCenter - maxLen)/2;
        return s.substring(st, st + maxLen);
    }
}
```

Time - O(n)   Space - O(n)