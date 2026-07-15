## [1752. Check if Array Is Sorted and Rotated](https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/)

Brute force - Concatenate the whole array and traverse through using window if found the original array then return true, this takes about O(n) and a space complexity of O(n).

Optimal fix, we do a modulus of length of array to warp around instead of creating extra space, code below.



```
class Solution {
    public boolean check(int[] nums) {
        int n = nums.length;
        int cnt = 1;
        for(int i = 1; i < 2 * n; i++){
            if(nums[(i-1) % n] <= nums[i % n]) cnt++;
            else cnt = 1;
            if(cnt == n) return true;
        }
        return n == 1;
    }
}```
```

Time - O(n) Space - O(1)


## [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

Sol - Traverse through the array using two pointer and check if nums[i] and nums[j-1] are different if yes then  replace the same nums[i] = nums[j] and increase the i by 1, in this way the duplicates will be removed.

Code below - >

```
class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        int i = 1;
        for(int j = 1; j < n; j++){
            if(nums[j] != nums[i-1]){
                nums[i] = nums[j];
                i++;
            }
        }
        return i;
    }
}
```

Time - O(n)  Space - O(1)

## [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

Sol - First we reverse the whole array then we reverse the array again till 0 to k-1 part then we rotate again k to n part of the array, no extra space used.

```
class Solution {
    private static void reverseToPoint(int[] arr, int start, int end) {
        while (start < end) {
            int temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;
            start++;
            end--;
        }
    }
    public void rotate(int[] nums, int k) {
        k = k % nums.length;
        reverseToPoint(nums, 0, nums.length-1);
        reverseToPoint(nums, 0, k-1);
        reverseToPoint(nums, k, nums.length-1);
    }
}
```

Time - O(n)  Space - O(1)

## [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)

Sol - Using two pointer, keep one pointer at the starting and iterate, if nums[i] != 0 then we swap the nums[l] with nums[i]

Code below -> 
```
class Solution {
    public void moveZeroes(int[] nums) {
        int n = nums.length;
        int l = 0;
        for(int i = 0; i < n; i++){
            if(nums[i] != 0){
                int temp = nums[l];
                nums[l] = nums[i];
                nums[i] = temp;
                l++;
            }
        }
    }
}
```

Time - O(n)  Space - O(1)

## [485. Max Consecutive Ones](https://leetcode.com/problems/max-consecutive-ones/)

Sol - Just iterate through and keep a cnt of the max no of ones
*Note - While returning compare the currentcnt and maxcnt*

Code below
```
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int n = nums.length;
        int cnt = 0;
        int tempcnt = 0;
        for(int i = 0; i < n; i++){
            if(nums[i] == 1){
                tempcnt++;
            }
            else{
                cnt = Math.max(cnt, tempcnt);
                tempcnt = 0;
            }
        }
        return cnt > tempcnt ? cnt : tempcnt;
    }
}
```

Time - O(n)  Space - O(1)

## [136. Single Number](https://leetcode.com/problems/single-number/)

Sol - Do XOR

Code below ->
```
class Solution {
    public int singleNumber(int[] nums) {
        int sum = nums[0];
        for(int i = 1; i < nums.length; i++){
            sum = sum ^ nums[i];
        }
        return sum;
    }
}
```

Time - O(n)  Space - O(1)

## [1. Two Sum](https://leetcode.com/problems/two-sum/)

Sol - Brute force is to double traverse the array and find the match.
optimal sol will be using hashmap, we keep a track of the target - nums[i] and the index,
every iteration we look in the map for the reminder of target-nums[i] in the map if that number exist we return the list or else we add this as an entry to the hashMap 

Code below 

```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> numMap = new HashMap<>();
        int n = nums.length;
        for(int i = 0; i < n; i++){
            int complement = target-nums[i];
            if(numMap.containsKey(complement)){
                return new int[] {numMap.get(complement), i};
            }
            numMap.put(nums[i], i);
        }
        return new int[]{};
    }
}
```

Time - O(n)  Space - O(n)

## [75. Sort Colors](https://leetcode.com/problems/sort-colors/)

Sol - Use Dutch National Flag algo, here we use a while loop till mid crosses high point and initialize low and mid with 0 and high with n - 1, and run the loop and if nums[mid] == 0 then we swap with the nums[low] and nums[mid] and increment both, if nums[mid] == 1 then just increment mid and if nums[mid] == 2 then swap nums[mid] with nums[high]

Code below - > 

```
class Solution {
    public void sortColors(int[] nums) {
        int n = nums.length;
        int i = 0,j = 0, k = n-1;
        while(j <= k){
            if(nums[j] == 0){
                int temp = nums[i];
                nums[i++] = nums[j];
                nums[j++] = temp;
            }
            else if(nums[j] == 1) j++;
            else{
                int temp = nums[j];
                nums[j] = nums[k];
                nums[k--] = temp;
            }
        }
    }
}
```

Time - O(n)  Space - O(1)

## [169. Majority Element](https://leetcode.com/problems/majority-element/)

Sol - Use Moore's voting Algo, where we run a for each loop and check the following
if cnt  = 0 then assign the target as nums
if num = target then we increment the cnt 
else we decrement the cnt and in the end return the target

Code Below -> 

```
class Solution {
    public int majorityElement(int[] nums) {
        int target = nums[0];
        int cnt = 0;
        for(int num: nums){
            if(cnt == 0) target = num;
            if(num == target) cnt++;
            else cnt--;
        }
        return target;
    }
}
```

Time - O(N)  Space - O(1)

## [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

Sol - Use Kadence Algo to find the max sum, what we do is take two var and initialize maxSum with nums[0] and currSum with 0, then we iterate through the array with condition that if currSum < 0 then we assign it again 0 and currSum += nums[i] and update the maxSum with whichever is the largest amongst both.

Code Below -> 

```
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = nums[0];
        int currSum = 0;
        for(int num: nums){
            if(currSum < 0) currSum = 0;
            currSum += num;
            maxSum = Math.max(currSum, maxSum);
        }
        return maxSum;
    }
}
```

Time - O(n)   Space - O(1)

## [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

Sol - Using greedy approach, use two pointer l and r where l is to buy and r is to sell, use a while loop till nums.length and on each iteration look for the condition where nums[l] < nums[r] if yes then we keep a local var to store the profit and then compare with the max profit and if nums[l] > nums[r] then thats our least number where we buy the stock so we assign l = r and in the end return maxProfit.

Code Below -> 
```
class Solution {
    public int maxProfit(int[] prices) {
        int buy = 0, sell = 1, maxProfit = 0;
        while(sell < prices.length){
            if(prices[buy] < prices[sell]){
                int currentProfit = prices[sell] - prices[buy];
                maxProfit = Math.max(maxProfit, currentProfit);
            }
            else{
                buy = sell;
            }
            sell++;
        }
        return maxProfit;
    }
}
```

Time - O(n)   Space - O(1)

## [2149. Rearrange Array Elements by Sign](https://leetcode.com/problems/rearrange-array-elements-by-sign/)

Sol - Use two Pointer, create an external array *Note - External Array to return the ans*, we keep a positive and negative var to keep track of the place - pos = 0 and neg = 1 and we iterate through the array on condition if nums[i]  >= 0 then we insert in the positive index and if nums[i] < 0 then we insert in the negative index of the ans array

Code Below -> 

```
class Solution {
    public int[] rearrangeArray(int[] nums) {
        int[] ans = new int[nums.length];
        int pos = 0, neg = 1;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] >= 0) {
                ans[pos] = nums[i];
                pos += 2;
            } else {
                ans[neg] = nums[i];
                neg += 2;
            }
        }
        return ans;
    }
}
```

Time - O(n)  Space  - O(n) *Extra space for returning the ans* 
