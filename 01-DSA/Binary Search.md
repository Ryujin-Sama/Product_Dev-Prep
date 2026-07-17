
## [704. Binary Search](https://leetcode.com/problems/binary-search/)

Sol - we take two pointers l and r, l at 0 and r at n - 1, so we iterate in the array until l <= r by checking the following and taking the mid point by l + r /2
if num[mid] > target then r = mid - 1
if num[mid] < target then l = mid + 1
and on the else we return the mid

Code Below -> 

```
class Solution {
    public int search(int[] nums, int target) {
        int l = 0, r = nums.length-1;
        while(l <= r){
            int mid = (l + r)/2;
            if(nums[mid] > target) r = mid - 1;
            else if(nums[mid] < target) l = mid + 1;
            else return mid;
        }
        return -1;
    }
}
```

Time - O(n)   Space - O(1)

## [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/)

Sol - Instead of returning -1 return the starting pointer.

Code below ->

```
class Solution {
    public int searchInsert(int[] nums, int target) {
        int l = 0, r = nums.length-1;
        while(l <= r){
            int mid = (l + r)/2;
            if(nums[mid] > target) r = mid - 1;
            else if(nums[mid] < target) l = mid + 1;
            else return mid;
        }
        return l;
    }
}
```

Time - O(n)  Space - O(1)