<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 1256. Rank Transform of an Array

</h>
</div>

<h2><a href="https://leetcode.com/problems/rank-transform-of-an-array" target = "_blank">1256. Rank Transform of an Array</a></h2><h3>Easy</h3><hr><p>Given an array of integers&nbsp;<code>arr</code>, replace each element with its rank.</p>

<p>The rank represents how large the element is. The rank has the following rules:</p>

<ul>
	<li>Rank is an integer starting from 1.</li>
	<li>The larger the element, the larger the rank. If two elements are equal, their rank must be the same.</li>
	<li>Rank should be as small as possible.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [40,10,20,30]
<strong>Output:</strong> [4,1,2,3]
<strong>Explanation</strong>: 40 is the largest element. 10 is the smallest. 20 is the second smallest. 30 is the third smallest.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [100,100,100]
<strong>Output:</strong> [1,1,1]
<strong>Explanation</strong>: Same elements share the same rank.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [37,12,28,9,100,56,80,5,12]
<strong>Output:</strong> [5,3,4,2,8,6,7,1,3]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup>&nbsp;&lt;= arr[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]">

<template #java>

```java
class Solution {
    public int[] arrayRankTransform(int[] arr) {
        int n = arr.length;
        HashMap<Integer, Integer> map = new HashMap<>();
        int[] ans = new int[n];
        for (int i = 0; i < n; i++)
            ans[i] = arr[i];
        Arrays.sort(ans);
        int y = 1;
        for (int i = 0; i < n; i++) {
            if (!map.containsKey(ans[i])) {
                map.put(ans[i], y);
                y++;
            }
        }
        int[] answer = new int[n];
        int k = 0;
        for (int i = 0; i < n; i++) {
            int x = map.get(arr[i]);
            answer[k] = x;
            k++;
        }
        return answer;
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
    vector<int> arrayRankTransform(vector<int>& arr) {
        int n = arr.size();
        vector<int> ans = arr;
        unordered_map<int, int> mp;

        sort(ans.begin(), ans.end());

        int rank = 1;
        for (int i = 0; i < n; i++) {
            if (!mp.count(ans[i])) {
                mp[ans[i]] = rank;
                rank++;
            }
        }

        vector<int> answer(n);
        for (int i = 0; i < n; i++)
            answer[i] = mp[arr[i]];
        return answer;
    }
};
```

</template>

</CodeTabs>
