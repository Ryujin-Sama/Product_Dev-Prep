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