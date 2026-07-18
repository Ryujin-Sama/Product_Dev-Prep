
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

## [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

Sol - we modify Binary search and we run twice to get the left most occurrence and the right most occurrence in such a way that, once we run to fight the last occurrence, second time the first 

Code below ->

```
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int left = BS(nums, target, true);
        int right = BS(nums, target, false);
        return new int[] {left, right};
    }
    public int BS(int[] nums, int target, boolean bias){
        int l = 0, r = nums.length-1;
        int i = -1;
        while(l <= r){
            int m = (l+r)/2;
            if(target > nums[m]) l = m + 1;
            else if(target < nums[m]) r = m - 1;
            else{
                i = m;
                if(bias) r = m - 1;
                else l = m + 1;
            }
        }
    return i;
    }
}
```

Time - O(n)   Space - O(1)