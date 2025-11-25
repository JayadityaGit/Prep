<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 1234. Number of Paths with Max Score

</h>
</div>

<h2><a href="https://leetcode.com/problems/number-of-paths-with-max-score" target = "_blank">1234. Number of Paths with Max Score</a></h2><h3>Hard</h3><hr><p>You are given a square <code>board</code>&nbsp;of characters. You can move on the board starting at the bottom right square marked with the character&nbsp;<code>&#39;S&#39;</code>.</p>

<p>You need&nbsp;to reach the top left square marked with the character <code>&#39;E&#39;</code>. The rest of the squares are labeled either with a numeric character&nbsp;<code>1, 2, ..., 9</code> or with an obstacle <code>&#39;X&#39;</code>. In one move you can go up, left or up-left (diagonally) only if there is no obstacle there.</p>

<p>Return a list of two integers: the first integer is the maximum sum of numeric characters you can collect, and the second is the number of such paths that you can take to get that maximum sum, <strong>taken modulo <code>10^9 + 7</code></strong>.</p>

<p>In case there is no path, return&nbsp;<code>[0, 0]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<pre><strong>Input:</strong> board = ["E23","2X2","12S"]
<strong>Output:</strong> [7,1]
</pre><p><strong class="example">Example 2:</strong></p>
<pre><strong>Input:</strong> board = ["E12","1X1","21S"]
<strong>Output:</strong> [4,2]
</pre><p><strong class="example">Example 3:</strong></p>
<pre><strong>Input:</strong> board = ["E11","XXX","11S"]
<strong>Output:</strong> [0,0]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= board.length == board[i].length &lt;= 100</code></li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]">

<template #java>

```java
import java.util.Arrays;
import java.util.List;

class Solution {
    private int sum[][];
    private int ways[][];
    private int mod = (int)(1e9 + 7);
    public int[] pathsWithMaxScore(List<String> board) {
        int n = board.size();
        int m = board.get(0).length();
        char matrix[][] = new char[n][m];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++)
                matrix[i][j] = board.get(i).charAt(j);
        }
        matrix[0][0] = '0';
        matrix[n - 1][m - 1] = '0';
        sum = new int[n + 1][m + 1];
        ways = new int[n + 1][m + 1];

        for (int current[] : sum)
            Arrays.fill(current, Integer.MIN_VALUE / 10);
        int dir[][] = {{0, -1}, {-1, 0}, {-1, -1}};

        sum[n - 1][m - 1] = 0;
        ways[n - 1][m - 1] = 1;
        for (int i = n - 1; i >= 0; i--) {
            for (int j = m - 1; j >= 0; j--) {
                if (matrix[i][j] == 'X')
                    continue;
                for (int dire[] : dir) {
                    int newRow = i + dire[0], newCol = j + dire[1];
                    if (newRow >= 0 && newRow < n && newCol >= 0 && newCol < m && matrix[newRow][newCol] != 'X') {
                        int newSum = sum[i][j] + matrix[newRow][newCol] - '0';
                        if (newSum > sum[newRow][newCol]) {
                            sum[newRow][newCol] = newSum;
                            ways[newRow][newCol] = ways[i][j];
                        } else if (newSum == sum[newRow][newCol])
                            ways[newRow][newCol] = (ways[newRow][newCol] + ways[i][j]) % mod;
                    }
                }
            }
        }
        if (ways[0][0] == 0)
            return new int[] {0, 0};
        return new int[] {sum[0][0], ways[0][0]};
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
    vector<vector<int>> sum;
    vector<vector<int>> ways;
    int mod = 1e9 + 7;

    vector<int> pathsWithMaxScore(vector<string>& board) {
        int n = board.size();
        int m = board[0].size();

        vector<vector<char>> matrix(n, vector<char>(m));
        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++)
                matrix[i][j] = board[i][j];

        matrix[0][0] = '0';
        matrix[n-1][m-1] = '0';

        sum.assign(n+1, vector<int>(m+1, INT_MIN/10));
        ways.assign(n+1, vector<int>(m+1, 0));

        int dir[3][2] = {{0, -1}, {-1, 0}, {-1, -1}};

        sum[n-1][m-1] = 0;
        ways[n-1][m-1] = 1;

        for (int i = n - 1; i >= 0; i--) {
            for (int j = m - 1; j >= 0; j--) {
                if (matrix[i][j] == 'X')
                    continue;

                for (auto &d : dir) {
                    int newRow = i + d[0];
                    int newCol = j + d[1];

                    if (newRow >= 0 && newRow < n &&
                        newCol >= 0 && newCol < m &&
                        matrix[newRow][newCol] != 'X') {

                        int newSum = sum[i][j] + (matrix[newRow][newCol] - '0');

                        if (newSum > sum[newRow][newCol]) {
                            sum[newRow][newCol] = newSum;
                            ways[newRow][newCol] = ways[i][j];
                        } else if (newSum == sum[newRow][newCol]) {
                            ways[newRow][newCol] = (ways[newRow][newCol] + ways[i][j]) % mod;
                        }
                    }
                }
            }
        }
        if (ways[0][0] == 0)
            return {0, 0};
        return {sum[0][0], ways[0][0]};
    }
};
```

</template>

</CodeTabs>
