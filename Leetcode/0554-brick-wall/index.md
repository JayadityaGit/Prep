<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 554. Brick Wall

</h>
</div>

<h2><a href="https://leetcode.com/problems/brick-wall" target = "_blank">554. Brick Wall</a></h2><h3>Medium</h3><hr><p>There is a rectangular brick wall in front of you with <code>n</code> rows of bricks. The <code>i<sup>th</sup></code> row has some number of bricks each of the same height (i.e., one unit) but they can be of different widths. The total width of each row is the same.</p>

<p>Draw a vertical line from the top to the bottom and cross the least bricks. If your line goes through the edge of a brick, then the brick is not considered as crossed. You cannot draw a line just along one of the two vertical edges of the wall, in which case the line will obviously cross no bricks.</p>

<p>Given the 2D array <code>wall</code> that contains the information about the wall, return <em>the minimum number of crossed bricks after drawing such a vertical line</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2025/01/17/a.png" style="width: 400px; height: 384px;" />
<pre>
<strong>Input:</strong> wall = [[1,2,2,1],[3,1,2],[1,3,2],[2,4],[3,1,2],[1,3,1,1]]
<strong>Output:</strong> 2
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> wall = [[1],[1],[1]]
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == wall.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= wall[i].length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= sum(wall[i].length) &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>sum(wall[i])</code> is the same for each row <code>i</code>.</li>
	<li><code>1 &lt;= wall[i][j] &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]"> <template #java>

```java
import java.util.HashMap;
import java.util.List;

class Solution {
    public int leastBricks(List<List<Integer >> wall) {
        int res = 0;
        HashMap<Long, Integer> freq = new HashMap<>();
        for (List<Integer> row : wall) {
            long sum = 0;
            for (int i = 0; i < row.size(); i++) {
                sum += row.get(i);
                if (i != row.size() - 1) {
                    freq.put(sum, freq.getOrDefault(sum, 0) + 1);
                    res = Math.max(res, freq.get(sum));
                }
            }
        }
        return wall.size() - res;
    }
}
```

</template>

<template #cpp>

```cpp
class Solution {
public:
    int leastBricks(vector<vector<int>>& wall) {
        unordered_map<long long, int> freq;
        int res = 0;
        for (const auto& row : wall) {
            long long sum = 0;
            for (int i = 0; i < row.size(); i++) {
                sum += row[i];
                if (i != row.size() - 1) {  // don't count the last edge
                    freq[sum]++;
                    res = max(res, freq[sum]);
                }
            }
        }
        return wall.size() - res;
    }
};
```

</template>

</CodeTabs>
