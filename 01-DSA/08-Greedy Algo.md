
## [455. Assign Cookies](https://leetcode.com/problems/assign-cookies/)

Sol - We sort both the array and check the cookie array with the kid array if it can be assignable and increment the count accordingly 

Code Below ->

```
class Solution {

    public int findContentChildren(int[] g, int[] s) {

        Arrays.sort(g);
        Arrays.sort(s);
        int n1 = g.length, n2 = s.length;
        int i = 0, j = 0;
        int cnt = 0;
        while(i < n1 && j < n2){
            if(s[j] >= g[i]){

                cnt++; j++; i++;
            }
            else{
                j++;
            }
        }
        return cnt;
    }
}
```

Time - O(N log N)  Space - O(1)
