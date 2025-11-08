<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 806. Domino and Tromino Tiling

</h>
</div>

<h2><a href="https://leetcode.com/problems/domino-and-tromino-tiling" target = "_blank">806. Domino and Tromino Tiling</a></h2><h3>Medium</h3><hr><p>You have two types of tiles: a <code>2 x 1</code> domino shape and a tromino shape. You may rotate these shapes.</p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/07/15/lc-domino.jpg" style="width: 362px; height: 195px;" />
<p>Given an integer n, return <em>the number of ways to tile an</em> <code>2 x n</code> <em>board</em>. Since the answer may be very large, return it <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>In a tiling, every square must be covered by a tile. Two tilings are different if and only if there are two 4-directionally adjacent cells on the board such that exactly one of the tilings has both squares occupied by a tile.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/07/15/lc-domino1.jpg" style="width: 500px; height: 226px;" />
<pre>
<strong>Input:</strong> n = 3
<strong>Output:</strong> 5
<strong>Explanation:</strong> The five different ways are show above.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> n = 1
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]"> <template #java>

```java
class Solution {
    static int mod = (int)(1e9 + 7);
    public int numTilings(int n) {
        long dp[] = new long[n + 3];
        dp[0] = 1;
        dp[1] = 2;
        dp[2] = 5;
        for (int i = 3; i < n; i++)
            dp[i] = (dp[i - 1] * 2 + dp[i - 3]) % mod;
        return (int)dp[n - 1];
    }
}
```

</template>

<template #cpp>

```cpp
class Solution {
public:
    int mod = 1e9 + 7;
    int numTilings(int n) {
        if (n <= 1) return 1;
        if (n == 2) return 2;
        if (n == 3) return 5;
        vector<int> dp(n + 1, 0);
        dp[0] = 1, dp[1] = 1, dp[2] = 2, dp[3] = 5;
        for (int i = 4; i <= n; i++) {
            dp[i] = (dp[i - 1] * 2 + long(dp[i - 3])) % mod;
        }
        return dp[n];
    }
};
```

</template>

</CodeTabs>
