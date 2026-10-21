
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


## [860. Lemonade Change](https://leetcode.com/problems/lemonade-change/)

Sol - we keep the count of fives and tens and keep conditions like if bill is tens then no fives > 0, and if bills is twenties then fives >0 and tens > 0 etc.

Code Below ->

```
class Solution {

    public boolean lemonadeChange(int[] bills) {

        int fives = 0, tens = 0;
        for(int i = 0; i < bills.length; i++){

            if(bills[i] == 5) fives++;
            else if(bills[i] == 10){

                if(fives < 1) return false;
                else{
                    fives--; tens++;
                }
            }

            else{

                if(fives > 0 && tens > 0){
                    tens--; fives--;
                }

                else if(fives >= 3) fives -= 3;
                else return false;
            }
        }
        return true;
    }
}
```

Time - O(N)  Space - O(1)


## [55. Jump Game](https://leetcode.com/problems/jump-game/)

Sol - We will try to take the most jump and add to a variable and keep checking if we can reach the length of the array if any case where i > maxlen of the array we return false

Code Below ->

```
class Solution {

    public boolean canJump(int[] nums) {

        int n = nums.length;
        int maxlen = 0;
        for(int i = 0; i < n; i++){

            if(i > maxlen) return false;
            maxlen = Math.max(maxlen, i + nums[i]);
            if(maxlen >= n - 1) return true;
        }
        return true;
    }
}
```

Time - O(N)   Space - O(1)