
## [39. Combination Sum](https://leetcode.com/problems/combination-sum/)

Sol - We will backtrack by recursion, with base case of ind == arr.length and target == 0 we will return till then if the arr(ind) <= target we will pick the same ind and first we will add the ind value to the temp list and if that particular recursive call backtracks we pop the element from the array and we go to the next ind.

Code Below ->

```
class Solution {

    public void findCombination(int ind, int[] candidates, int target, List<List<Integer>> ans, List<Integer> ds){

        if(ind == candidates.length){
            if(target == 0) ans.add(new ArrayList<>(ds));
            return;
        }

        if(candidates[ind] <= target){

            ds.add(candidates[ind]);
            findCombination(ind, candidates, target-candidates[ind],ans, ds);
            ds.remove(ds.size()-1);
        }

        findCombination(ind + 1, candidates, target,ans, ds);
    }

    public List<List<Integer>> combinationSum(int[] candidates, int target) {

        List<List<Integer>> ans = new ArrayList<>();
        findCombination(0, candidates, target, ans, new ArrayList<>());
        return ans;
    }
}
```

Time -
$$O(2^{target/min} \times k)$$

- **$2^{target/min}$:** In the worst-case scenario, every step either includes the smallest possible candidate ($min$) or moves to the next index. This creates a decision tree of depth up to $target/min$, leading to an exponential number of possible combinations and recursive paths.
    
- **$k$:** This represents the average length of each valid combination, which is the time it takes to copy each valid result list into the final answer array (`ans.add(new ArrayList<>(ds))`).

Space - 
**$O(target/min)$**.


## [40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

Sol -  We will follow the same backtrack as combination sum 1 but with a different condition if we find i > ind and arr(i) == arr(i-1) then we continue the loop wont iterate through it 

Code Below ->

```
class Solution {

    public void findCombination(int ind, int[] candidates, int target, List<List<Integer>> ans, List<Integer> ds){

        if(target == 0){
            ans.add(new ArrayList<>(ds));
            return;
        }

        for(int i = ind; i < candidates.length; i++){

            if(i > ind && candidates[i] == candidates[i-1]) continue;
            if(candidates[i] > target) break;
            ds.add(candidates[i]);
            findCombination(i + 1, candidates, target - candidates[i], ans, ds);
            ds.remove(ds.size()-1);
        }
    }

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {

        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(candidates);
        findCombination(0, candidates, target, ans, new ArrayList<>());
        return ans;
    }
}
```

Time -
$$O(2^N \times k)$$

- **$2^N$:** Where $N$ is the number of elements in the `candidates` array. Unlike the first version where elements could be chosen infinitely, here each element can be chosen at most once. This transforms the problem into finding subsets (or combinations of a fixed set), resulting in $2^N$ possible subsets in the worst-case scenario. Sorting the array takes $O(N \log N)$, which is dominated by the exponential recursive exploration.
    
- **$k$:** The average length of each valid combination, representing the time it takes to copy the valid list into the final answer array (`ans.add(new ArrayList<>(ds))`).

Space - $O(N)$


## [78. Subsets](https://leetcode.com/problems/subsets/)

Sol - We will backtrack with include and exclude mechanism until ind >= arr.length;

Code Below ->

```
class Solution {

    public void findSubsets(int ind, int[] nums, List<List<Integer>> res, List<Integer> ds){

        if(ind >= nums.length){

            res.add(new ArrayList<>(ds));
            return;
        }

        ds.add(nums[ind]);
        findSubsets(ind + 1, nums, res, ds);
        ds.remove(ds.size() - 1);
        findSubsets(ind + 1, nums, res, ds);
    }

    public List<List<Integer>> subsets(int[] nums) {

        List<List<Integer>> res = new ArrayList<>();
        findSubsets(0, nums, res, new ArrayList<>());
        return res;
    }
}
```

Time - 
		$O(2^n \cdot n)$
There are $2^n$ total subsets (leaves of the tree), and copying each subset of average size $n$ into the result list takes $O(n)$ time.

Space - 
		$O(n)$
The maximum depth of the recursion tree is equal to the length of the array (`n`), which dictates the maximum size of the call stack and temporary data structure `ds`.


## [90. Subsets II](https://leetcode.com/problems/subsets-ii/)

Sol - we follow the same solution of subset 1 just we run a while loop to skip the index with same value

Code Below ->

```
class Solution {

    public void findSubsets(int ind, int[] nums, List<List<Integer>> res, List<Integer> ds){

        if(ind >= nums.length){

            res.add(new ArrayList<>(ds));
            return;
        }

        ds.add(nums[ind]);
        findSubsets(ind + 1, nums, res, ds);
        ds.remove(ds.size() - 1);
        while(ind < nums.length - 1 && nums[ind] == nums[ind + 1]) ind++;
        findSubsets(ind + 1, nums, res, ds);
    }

    public List<List<Integer>> subsetsWithDup(int[] nums) {

        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);
        findSubsets(0, nums, res, new ArrayList<>());
        return res;
    }
}
```


Time - 
		$O(2^n \cdot n)$
- **Sorting:** $O(n \log n)$ to sort the array initially so that duplicates are adjacent.
- **Recursion Tree Exploration:** Although the `while` loop skips duplicate elements to avoid generating identical subsets, in the worst-case scenario (where all elements are distinct), the number of subsets remains $2^n$.
- **Subset Construction:** At each of the $2^n$ leaves, copying the subset of size up to $n$ into the result list (`res.add(new ArrayList<>(ds))`) takes $O(n)$ time.
- **Overall Time:** $O(n \log n + 2^n \cdot n)$, which simplifies to **$O(2^n \cdot n)$**

Space -
		 $O(2^n \cdot n)$
- **Recursion Stack:** The maximum depth of the recursion tree is $O(n)$, which is the space required for the call stack and the temporary list (`ds`).
- **Result Storage (`res`):** Storing all the unique subsets takes up space proportional to the number of unique subsets multiplied by their average length. In the worst case (all elements unique), this is $O(2^n \cdot n)$. If there are heavy duplicates, it will be less, but space complexity bounds are typically expressed in terms of the worst case
