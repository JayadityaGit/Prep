<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 494. Target Sum

</h>
</div>

<h2><a href="https://leetcode.com/problems/target-sum" target = "_blank">494. Target Sum</a></h2><h3>Medium</h3><hr><p>You are given an integer array <code>nums</code> and an integer <code>target</code>.</p>

<p>You want to build an <strong>expression</strong> out of nums by adding one of the symbols <code>&#39;+&#39;</code> and <code>&#39;-&#39;</code> before each integer in nums and then concatenate all the integers.</p>

<ul>
	<li>For example, if <code>nums = [2, 1]</code>, you can add a <code>&#39;+&#39;</code> before <code>2</code> and a <code>&#39;-&#39;</code> before <code>1</code> and concatenate them to build the expression <code>&quot;+2-1&quot;</code>.</li>
</ul>

<p>Return the number of different <strong>expressions</strong> that you can build, which evaluates to <code>target</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,1,1,1,1], target = 3
<strong>Output:</strong> 5
<strong>Explanation:</strong> There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1], target = 1
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 20</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>0 &lt;= sum(nums[i]) &lt;= 1000</code></li>
	<li><code>-1000 &lt;= target &lt;= 1000</code></li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]"> <template #java>

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int n = nums.length;
        int sum = 0;
        for (int i = 0; i < n; i++)
            sum += nums[i];
        if (sum - target < 0)
            return 0;
        else if ((sum - target) % 2 != 0)
            return 0;
        else {
            int target1 = (sum - target)  /  2;
            int dp[][] = new int[n + 1][target1 + 1];
            for (int temp[] : dp)
                Arrays.fill(temp, -1);
            return solve(n - 1, nums, target1, dp);
        }
    }
    private int solve(int ind, int arr[], int target, int dp[][]) {
        if (ind == 0) {
            if (target == 0 && arr[0] == 0)
                return 2;
            if (target == 0 || arr[0] == target)
                return 1;
            return 0;
        }
        if (dp[ind][target] != -1)
            return dp[ind][target];
        int not_take = solve(ind - 1, arr, target, dp);
        int take = 0;
        if (arr[ind] <= target)
            take = solve(ind - 1, arr, target - arr[ind], dp);
        return dp[ind][target] = (take + not_take);
    }
}
```

</template>

<template #cpp>

```cpp
class Solution {
public:
    int f(int i , vector<int> &arr, int target, vector<vector<int>>&dp){
        if (i == 0) {
            if (target == 0 && arr[0] != 0) return 1;
            if (target == 0 && arr[0] == 0) return 2; 
            if (arr[0] == target) return 1;
            return 0;
        }
        if (dp[i][target] != -1) return dp[i][target];
        int notTake = f(i -1, arr, target, dp);
        int take = 0;
        if(arr[i] <= target) take = f(i-1, arr, target - arr[i], dp);
        return dp[i][target] = take + notTake;
    }
    int countPartitions(vector<int>& arr, int d) {
        int n = arr.size();
        int sum = accumulate(arr.begin(), arr.end(), 0ll);
        int s2 = (sum - d);
        if(s2 < 0 || s2 % 2 == 1) return 0;
        s2 /= 2;
        vector<vector<int>> dp(n, vector<int> (s2 + 1, -1));
        return f(n - 1, arr, s2, dp);
    }
    int findTargetSumWays(vector<int>& nums, int target) {
        return countPartitions(nums, target);
    }
};
```

</template>

</CodeTabs>
