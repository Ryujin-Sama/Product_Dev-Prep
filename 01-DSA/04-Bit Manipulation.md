
## [231. Power of Two](https://leetcode.com/problems/power-of-two/)

Sol - Using Bit we can understand one thing binary of n and n-1 == 0 always so we use the same way 

Code Below ->

```
class Solution {
    public boolean isPowerOfTwo(int n) {
        if(n<=0) return false;
        return ((n&(n-1))==0);
    }
}
```

Time - O(1)  Space - O(1)