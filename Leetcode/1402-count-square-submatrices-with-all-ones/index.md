<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 1402. Count Square Submatrices with All Ones

</h>
</div>

<h2><a href="https://leetcode.com/problems/count-square-submatrices-with-all-ones" target = "_blank">1402. Count Square Submatrices with All Ones</a></h2><h3>Medium</h3><hr><p>Given a <code>m * n</code> matrix of ones and zeros, return how many <strong>square</strong> submatrices have all ones.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> matrix =
[
&nbsp; [0,1,1,1],
&nbsp; [1,1,1,1],
&nbsp; [0,1,1,1]
]
<strong>Output:</strong> 15
<strong>Explanation:</strong> 
There are <strong>10</strong> squares of side 1.
There are <strong>4</strong> squares of side 2.
There is  <strong>1</strong> square of side 3.
Total number of squares = 10 + 4 + 1 = <strong>15</strong>.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> matrix = 
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
<strong>Output:</strong> 7
<strong>Explanation:</strong> 
There are <b>6</b> squares of side 1.  
There is <strong>1</strong> square of side 2. 
Total number of squares = 6 + 1 = <b>7</b>.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length&nbsp;&lt;= 300</code></li>
	<li><code>1 &lt;= arr[0].length&nbsp;&lt;= 300</code></li>
	<li><code>0 &lt;= arr[i][j] &lt;= 1</code></li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]">

<template #java>

```java
class Solution {
    static int prefix[][];
    public int countSquares(int[][] matrix) {
        int n = matrix.length, m = matrix[0].length;
        prefix = new int[n + 1][m + 1];
        build(matrix);
        int count = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == 1) {
                    int x1 = i, y1 = j, x2 = i, y2 = j;
                    while (x2 < n && y2 < m) {
                        int req_area = (y2 - y1 + 1) * (x2 - x1 + 1);
                        int query = query(x1 + 1, y1 + 1, x2 + 1, y2 + 1);
                        if (query == req_area)
                            count++;
                        x2++;
                        y2++;
                    }
                }
            }
        }
        return count;
    }
    static int query(int x1, int y1, int x2, int y2) {
        return prefix[x2][y2] + prefix[x1 - 1][y1 - 1] - prefix[x1 - 1][y2] - prefix[x2][y1 - 1];
    }
    static void build(int matrix[][]) {
        int n = matrix.length;
        int m = matrix[0].length;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++)
                prefix[i][j] = matrix[i - 1][j - 1] + prefix[i][j - 1] + prefix[i - 1][j] - prefix[i - 1][j - 1];
        }
    }
}
```

</template>

<template #cpp>

```cpp
class Solution {
public:
    vector<vector<int>> prefix;

    int countSquares(vector<vector<int>>& matrix) {
        int n = matrix.size(), m = matrix[0].size();
        prefix.assign(n + 1, vector<int>(m + 1, 0));

        build(matrix);

        int count = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == 1) {
                    int x1 = i, y1 = j, x2 = i, y2 = j;
                    while (x2 < n && y2 < m) {
                        int req_area = (x2 - x1 + 1) * (y2 - y1 + 1);
                        int q = query(x1 + 1, y1 + 1, x2 + 1, y2 + 1);
                        if (q == req_area)
                            count++;
                        x2++;
                        y2++;
                    }
                }
            }
        }
        return count;
    }

private:
    int query(int x1, int y1, int x2, int y2) {
        return prefix[x2][y2]
             + prefix[x1 - 1][y1 - 1]
             - prefix[x1 - 1][y2]
             - prefix[x2][y1 - 1];
    }

    void build(const vector<vector<int>>& matrix) {
        int n = matrix.size(), m = matrix[0].size();
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                prefix[i][j] = matrix[i - 1][j - 1]
                             + prefix[i - 1][j]
                             + prefix[i][j - 1]
                             - prefix[i - 1][j - 1];
            }
        }
    }
};
```

</template>

</CodeTabs>
