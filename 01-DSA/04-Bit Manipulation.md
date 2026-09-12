
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


## [2220. Minimum Bit Flips to Convert Number](https://leetcode.com/problems/minimum-bit-flips-to-convert-number/)

Sol - Use XOR and count the 1s

Code Below ->

```
class Solution {
    public int minBitFlips(int start, int goal) {
       return Integer.bitCount(start ^ goal);
    }
}
```

Time - O(1)    Space - O(1)

## [136. Single Number](https://leetcode.com/problems/single-number/)

Sol - Use XOR

Code Below ->

```
class Solution {
    public int singleNumber(int[] nums) {
        int xor = 0;

        for(int i = 0; i < nums.length; i++){
            xor ^= nums[i];
        }
        return xor;
    }
}
```

Time - O(n)   Space - O(1)


## [78. Subsets](https://leetcode.com/problems/subsets/)

Sol - we find the total of subset with 1 << n - 2 ^ n  then we run two loop one with i = 0 to i < 2^n and second one from j = 0 to j < n, and we find if (i & (1 << j)) != 0, then we add that nums(j) in the list.

Code Below -> 

```
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        int n = nums.length;
        int subq = 1 << n;

        for(int i = 0; i < subq; i++){
            List<Integer> subset = new ArrayList<>();
            for(int j = 0; j < n; j++){
                if((i & (1 << j)) != 0) subset.add(nums[j]);
            }
            res.add(subset);
        }
        return res;
    }
}
```

Time - O( n x 2 ^ n)   Space - O(2^n)


## [204. Count Primes](https://leetcode.com/problems/count-primes/)

Sol - Use  **Sieve of Eratosthenes** 2 loops, one from i = 2 to  i * i < n and second is from j = i * i to j < n,

Code below -> 

```
class Solution {
    public int countPrimes(int n) {

        if(n < 2) return 0;
        int cnt = 0;
        boolean[] comp = new boolean[n];
        for(int i = 2; i * i < n; i++){
            if(!comp[i]){
                for(int j = i * i; j < n; j += i) comp[j] = true;
            }
        }
        for(int i = 2; i < n; i++) if(!comp[i]) cnt++;
        return cnt;
    }
}
```

Time - O(n(log(log n)))    Space - O(n)


## [50. Pow(x, n)](https://leetcode.com/problems/powx-n/)

Sol - - **Odd Exponent (`nn % 2 != 0`):** Multiplies the running result (`ans`) by the current base `x` and reduces the exponent by 1. **Even Exponent (`nn % 2 == 0`):** Squares the base (`x *= x`) and cuts the exponent in half (`nn /= 2`).

Code Below ->

```
class Solution {
    public double myPow(double x, int n) {
        double nn = n;
        if(n < 0) nn = -1 * nn;
        double ans = 1.0;

        while(nn > 0){
            if(nn % 2 != 0){
                ans *= x;
                nn -= 1;
            }
            else{
                x *= x;
                nn /= 2;
            }
        }
        if(n < 0) return 1/ans;
        return ans;
    }
}
```

Time - O(log n)   Space - O(1)
