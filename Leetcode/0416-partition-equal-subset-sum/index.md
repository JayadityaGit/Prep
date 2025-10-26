<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 416. Partition Equal Subset Sum

</h>
</div>

<h2><a href="https://leetcode.com/problems/partition-equal-subset-sum" target = "_blank">416. Partition Equal Subset Sum</a></h2><h3>Medium</h3><hr><p>Given an integer array <code>nums</code>, return <code>true</code> <em>if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or </em><code>false</code><em> otherwise</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,5,11,5]
<strong>Output:</strong> true
<strong>Explanation:</strong> The array can be partitioned as [1, 5, 5] and [11].
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3,5]
<strong>Output:</strong> false
<strong>Explanation:</strong> The array cannot be partitioned into equal sum subsets.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 200</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' },
  { name: 'Java', slot: 'java' }
]">

<template #java>

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        int n = nums.length;
        for (int ele : nums)
            sum += ele;
        if (sum % 2 == 1)
            return false;
        else {
            int target = sum / 2;
            int dp[][] = new int[n + 1][target + 1];
            for (int temp[] : dp)
                Arrays.fill(temp, -1);
            if (solve(n - 1, nums, target, dp) == 1)
                return true;
            return false;
        }
    }
    public static int solve(int ind, int arr[], int target, int dp[][]) {
        if (target == 0)
            return 1;
        if (ind == 0) {
            if (arr[0] == target)
                return 1;
            return 0;
        }
        if (dp[ind][target] != -1)
            return dp[ind][target];
        int not_take = solve(ind - 1, arr, target, dp);
        int take = 0;
        if (arr[ind] <= target)
            take = solve(ind - 1, arr, target - arr[ind], dp);
        return dp[ind][target] = (take | not_take);
    }
}
```

</template>

<template #cpp>

```cpp
class Solution {
public:
    bool f(int i, int sum, vector<int>& arr, vector<vector<int>> &dp){
        if(sum == 0) return 1;
        if(i == 0) return (arr[0] == sum);
        if(dp[i][sum] != -1) return dp[i][sum];
        bool notTake = f(i - 1, sum, arr, dp);
        bool Take = false;
        if(arr[i] <= sum) Take = f(i - 1, sum - arr[i], arr, dp);
        return dp[i][sum] = notTake | Take;
    }
    bool canPartition(vector<int>& nums) {
        int sum = accumulate(nums.begin(), nums.end(), 0LL);
        if(sum & 1) return false;
        int n = nums.size();
        int target = sum / 2;
        vector<vector<int>> dp(n, vector<int> (target + 1, -1));
        return f(n -1, target, nums, dp);
    }
};
```

</template>

</CodeTabs>
