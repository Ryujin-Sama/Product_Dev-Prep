
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


## [29. Divide Two Integers](https://leetcode.com/problems/divide-two-integers/)

Sol - Use 2^cnt logic where the dividend can be split with the divisor eg for dividend = 22 and divisor = 3 , 22 = (3 x 7) = (3 x 2^1) + (3 x 2^2) + (3 x 2^0)
so we keep a counter to keep increasing the power and we keep on reducing the dividend once the power become greater we return the count 

Code Below ->

```
class Solution {
    public int divide(int dividend, int divisor) {
        if(dividend == divisor) return 1;
        if(dividend == Integer.MIN_VALUE && divisor == -1) return Integer.MAX_VALUE;
        if(divisor == 1) return dividend;
        if(dividend == -1) return -dividend;
        boolean sign = true;
        if(dividend > 0 && divisor < 0) sign = false;
        if(dividend < 0 && divisor > 0) sign = false;
        int ans = 0;

        long n = Math.abs((long)dividend), d = Math.abs((long)divisor);
        while( n >= d){
            int p = 0;
            while(n >= d<<(p+1)) p++;
            ans += 1<<p;
            n -= d<<p;
        }
        // if(ans>=Math.pow(2,31) && sign) return Integer.MAX_VALUE;
        // if(ans>=Math.pow(2,31) && !sign) return Integer.MIN_VALUE;
        return sign ? ans : -ans;
    }
}
```

Time - O(log n)   Space - O(1)