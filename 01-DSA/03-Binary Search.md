
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


## [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

Sol - We can use Binary Search on this with bit modification, On a rotated array there is one thing we can notice i.e. either right side or the left side before the rotate the array would be completely sorted. so we use that to search the same.

Code Below -> 

```
class Solution {
    public int search(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while(low <= high){
            int mid = (low + high) >> 1;
            if(nums[mid] == target) return mid;
            if(nums[low] <= nums[mid]){
                if(nums[low] <= target && target < nums[mid]) high = mid - 1;
                else low = mid + 1;
            }
            else{
                if(nums[mid] < target && target <= nums[high]) low = mid + 1;
                else high = mid - 1;
            }
        }
        return -1;
    }
}
```

Time - O(log n)  Space - O(1)


## [81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

Sol - We modify the fix for the above question with a condition to skip the same elements 

Code Below -> 

```
class Solution {
    public boolean search(int[] arr, int k) {
        int n = arr.length;
        int low = 0, high = n - 1;
        while(low <= high){
            int mid = (low + high) / 2;
            if(arr[mid] == k) return true;
            if(arr[low] == arr[mid] && arr[mid] == arr[high]){
                low++; high--;
                continue;
            }
            if(arr[low] <= arr[mid]){
                if(arr[low] <= k && k <= arr[mid]) high = mid - 1;
                else low = mid + 1;
            }
            else{
                if(arr[mid] <= k && k <= arr[high]) low = mid + 1;
                else high = mid - 1;
            }
        }
        return false;
    }
}
```

Time - O(log n)    Space - O(1)

## [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

Sol - We run a while loop from low to high and first condition is to check if nums(low) <= nums(high) this means the array is sorted and we can return the first element, or else we check which half of the array is sorted by checking if nums(low) <= nums(mid) this means the left side is sorted, and we take low = mid + 1 and proceed and on each increment we check for the min value with nums(low) and if the right side is sorted then we decrement high = mid - 1 and compare the min for it with nums(min)

Code Below -> 

```
class Solution {
    public int findMin(int[] nums) {
        int low = 0, high = nums.length - 1;
        int ans = Integer.MAX_VALUE;
        while(low <= high){
            int mid = (low + high) >> 1;
            if(nums[low] <= nums[high]){
                ans = Math.min(ans, nums[low]);
                break;
            }
            if(nums[low] <= nums[mid]){
                ans = Math.min(ans, nums[low]);
                low = mid + 1;
            }
            else{
                high = mid - 1;
                ans = Math.min(ans, nums[mid]);
            }
        }
        return ans;
    }
}
```

Time - O(log n)    Space - O(1)


## [540. Single Element in a Sorted Array](https://leetcode.com/problems/single-element-in-a-sorted-array/)

Sol - Modify the Binary Search - We check for the condition where if mid % 2 == 0 or mid % 2 == 1 and if yes we check further check if == 0 then nums(mid) == nums(mid + 1) if  == 1 then check for nums(mid) == nums(mid - 1) if these condition matches then 
Condition below -> 
*if((mid % 2 == 0 && nums(mid) == nums(mid + 1)) || (mid % 2 == 1 && nums(mid) == nums(mid - 1)))*
then left = mid + 1 or else right = mid, at the end return nums(left)

Code Below -> 

```
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int right = nums.length - 1, left = 0;
        while(left < right){
            int mid = (left + right) >> 1;
            if((mid % 2 == 0 && nums[mid] == nums[mid + 1]) || (mid % 2 == 1 && nums[mid] == nums[mid - 1])) left = mid + 1;
            else right = mid;
        }
        return nums[left];
    }
}
```

Time - O(log n)  Space - O(1)


## [162. Find Peak Element](https://leetcode.com/problems/find-peak-element/)

Sol - Modify the Binary Search with following conditions. few pre condition if n == 1 then we can check nums(0) and nums(1) and return 0, and nums(n-1) and nums(n-2) and return n-1
we take  l = 1, and r = n - 2 and run the Binary search with a condition
*if(nums(m) > nums(m-1) && nums(m) > nums(m+1))* and return m rest if else and else we do the changes for l and r 

Code Below -> 
```
class Solution {
    public int findPeakElement(int[] nums) {
        int n = nums.length;
        if(n == 1 || nums[0] > nums[1]) return 0;
        if(nums[n-1] > nums[n-2]) return n-1;
        int l = 1, r = n-2;
        while(l <= r){
            int m = (l + r) >> 1;
            if(nums[m] > nums[m-1] && nums[m] > nums[m+1]) return m;
            else if(nums[m] > nums[m-1]) l = m + 1;
            else r = m-1;
        }
        return 0;
    }
}
```

Time - O(log n)   Space - O(1)

## [1482. Minimum Number of Days to Make m Bouquets](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/)

Sol - We use Binary search, find the min and max from the array and use binary search on min and max, and run a inner loop to find if it's possible to make the bonquest 

Code Below ->

```
class Solution {
    public boolean possible(int[] bloomDay, int mid, int m, int k){
        int cnt = 0;
        int bcnt = 0;
        for(int day: bloomDay){
            if(day <= mid){
                cnt++;
                if(cnt == k){
                    bcnt++;
                    cnt = 0;
                }
            }
            else cnt = 0;
        }
        return bcnt >= m;
    }

    public int minDays(int[] bloomDay, int m, int k) {
        if((long)m * k > bloomDay.length) return -1;
        int s = Integer.MAX_VALUE, e = Integer.MIN_VALUE;
        for(int day: bloomDay){
            s = Math.min(s, day);
            e = Math.max(e, day);
        }
        int ans = -1;
        while(s <= e){
            int mid = (s+e)/2;
            if(possible(bloomDay, mid, m, k)){
                ans = mid;
                e = mid - 1;
            }
            else s = mid + 1;
        }
        return ans;
    }
}
```

Time - O(n log (max-min+1))  Space - O(1)