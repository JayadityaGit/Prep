<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 1500. Count Largest Group

</h>
</div>

<h2><a href="https://leetcode.com/problems/count-largest-group" target = "_blank">1500. Count Largest Group</a></h2><h3>Easy</h3><hr><p>You are given an integer <code>n</code>.</p>

<p>Each number from <code>1</code> to <code>n</code> is grouped according to the sum of its digits.</p>

<p>Return <em>the number of groups that have the largest size</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> n = 13
<strong>Output:</strong> 4
<strong>Explanation:</strong> There are 9 groups in total, they are grouped according sum of its digits of numbers from 1 to 13:
[1,10], [2,11], [3,12], [4,13], [5], [6], [7], [8], [9].
There are 4 groups with largest size.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> n = 2
<strong>Output:</strong> 2
<strong>Explanation:</strong> There are 2 groups [1], [2] of size 1.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]">

<template #java>

```java
class Solution {
    public int countLargestGroup(int n) {
        int freq[] = new int[100];
        for (int i = 1; i <= n; i++)
            freq[solve(i)]++;
        int maxi = 0, count = 0;
        for (int ele : freq)
            maxi = Math.max(maxi, ele);
        for (int ele : freq)
            if (ele == maxi)
                count++;
        return count;
    }
    private int solve(int n) {
        int temp = n;
        int sum = 0;
        while (temp > 0) {
            sum += temp % 10;
            temp /= 10;
        }
        return sum;
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
    int countLargestGroup(int n) {
        int freq[100] = {0};
        for (int i = 1; i <= n; i++)
            freq[digitSum(i)]++;
        int maxi = 0, count = 0;
        for (int i = 0; i < 100; i++)
            maxi = max(maxi, freq[i]);
        for (int i = 0; i < 100; i++)
            if (freq[i] == maxi)
                count++;
        return count;
    }

private:
    int digitSum(int x) {
        int sum = 0;
        while (x > 0) {
            sum += x % 10;
            x /= 10;
        }
        return sum;
    }
};
```

</template>

</CodeTabs>
