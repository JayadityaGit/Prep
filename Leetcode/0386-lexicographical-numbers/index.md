<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 386. Lexicographical Numbers

</h>
</div>

<h2><a href="https://leetcode.com/problems/lexicographical-numbers" target = "_blank">386. Lexicographical Numbers</a></h2><h3>Medium</h3><hr><p>Given an integer <code>n</code>, return all the numbers in the range <code>[1, n]</code> sorted in lexicographical order.</p>

<p>You must write an algorithm that runs in&nbsp;<code>O(n)</code>&nbsp;time and uses <code>O(1)</code> extra space.&nbsp;</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<pre><strong>Input:</strong> n = 13
<strong>Output:</strong> [1,10,11,12,13,2,3,4,5,6,7,8,9]
</pre><p><strong class="example">Example 2:</strong></p>
<pre><strong>Input:</strong> n = 2
<strong>Output:</strong> [1,2]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' },
  { name: 'Java', slot: 'java' }
]">

<template #java>

```java
class Solution {
    public List<Integer> lexicalOrder(int n) {
        List<Integer> res = new ArrayList<>(n);
        int current = 1;
        for (int i = 0; i < n; i++) {
            res.add(current);
            if (current * 10 <= n)
                current *= 10;
            else {
                while (current % 10 == 9 || current + 1 > n)
                    current /= 10;
                current++;
            }
        }
        return res;
    }
}
```

</template>

<template #cpp>

```cpp
class Solution {
private:
    void solve(int currNum, int n, vector<int> &res){
        if(currNum > n) return;
        res.push_back(currNum);
        for(int i = 0; i <= 9; i++){
            int nextNum = currNum * 10 + i;
            if(nextNum > n) return; 
            solve(nextNum, n, res);
        }
    }
public:
    vector<int> lexicalOrder(int n) {
        vector<int> res;
        for(int i = 1; i <= 9; i++){
            solve(i, n, res);
        }
        return res;
    }
};
```

</template>

</CodeTabs>
