<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 1295. Minimum Garden Perimeter to Collect Enough Apples

</h>
</div>

<h2><a href="https://leetcode.com/problems/minimum-garden-perimeter-to-collect-enough-apples" target = "_blank">1295. Minimum Garden Perimeter to Collect Enough Apples</a></h2><h3>Medium</h3><hr><p>In a garden represented as an infinite 2D grid, there is an apple tree planted at <strong>every</strong> integer coordinate. The apple tree planted at an integer coordinate <code>(i, j)</code> has <code>|i| + |j|</code> apples growing on it.</p>

<p>You will buy an axis-aligned <strong>square plot</strong> of land that is centered at <code>(0, 0)</code>.</p>

<p>Given an integer <code>neededApples</code>, return <em>the <strong>minimum perimeter</strong> of a plot such that <strong>at least</strong></em><strong> </strong><code>neededApples</code> <em>apples are <strong>inside or on</strong> the perimeter of that plot</em>.</p>

<p>The value of <code>|x|</code> is defined as:</p>

<ul>
	<li><code>x</code> if <code>x &gt;= 0</code></li>
	<li><code>-x</code> if <code>x &lt; 0</code></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2019/08/30/1527_example_1_2.png" style="width: 442px; height: 449px;" />
<pre>
<strong>Input:</strong> neededApples = 1
<strong>Output:</strong> 8
<strong>Explanation:</strong> A square plot of side length 1 does not contain any apples.
However, a square plot of side length 2 has 12 apples inside (as depicted in the image above).
The perimeter is 2 * 4 = 8.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> neededApples = 13
<strong>Output:</strong> 16
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> neededApples = 1000000000
<strong>Output:</strong> 5040
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= neededApples &lt;= 10<sup>15</sup></code></li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]">

<template #java>

```java
class Solution {
    public long minimumPerimeter(long neededApples) {
        long low = 1;
        long high = (int)(1e5);
        long ans = -1;
        while (low <= high) {
            long mid = low + (high - low) / 2;
            if (ok(mid, neededApples)) {
                ans = mid;
                high = mid - 1;
            } else
                low = mid + 1;
        }
        return ans * 8;
    }

    private static boolean ok(long mid, long need) {
        long total = 2L * mid * (2L * mid * mid + 3L * mid + 1);
        return total >= need;
    }
}
```

</template>

<template #cpp>

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    long long minimumPerimeter(long long neededApples) {
        long long low = 1;
        long long high = 100000;
        long long ans = -1;
        while (low <= high) {
            long long mid = low + (high - low) / 2;
            if (ok(mid, neededApples)) {
                ans = mid;
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }
        return ans * 8;
    }

private:
    static bool ok(long long mid, long long need) {
        long long total = 2LL * mid * (2LL * mid * mid + 3LL * mid + 1LL);
        return total >= need;
    }
};
```

</template>

</CodeTabs>
