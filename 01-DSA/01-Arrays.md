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