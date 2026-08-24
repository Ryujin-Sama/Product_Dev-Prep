
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