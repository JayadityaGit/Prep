<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 812. Rotate String

</h>
</div>

<h2><a href="https://leetcode.com/problems/rotate-string" target = "_blank">812. Rotate String</a></h2><h3>Easy</h3><hr><p>Given two strings <code>s</code> and <code>goal</code>, return <code>true</code> <em>if and only if</em> <code>s</code> <em>can become</em> <code>goal</code> <em>after some number of <strong>shifts</strong> on</em> <code>s</code>.</p>

<p>A <strong>shift</strong> on <code>s</code> consists of moving the leftmost character of <code>s</code> to the rightmost position.</p>

<ul>
	<li>For example, if <code>s = &quot;abcde&quot;</code>, then it will be <code>&quot;bcdea&quot;</code> after one shift.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<pre><strong>Input:</strong> s = "abcde", goal = "cdeab"
<strong>Output:</strong> true
</pre><p><strong class="example">Example 2:</strong></p>
<pre><strong>Input:</strong> s = "abcde", goal = "abced"
<strong>Output:</strong> false
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, goal.length &lt;= 100</code></li>
	<li><code>s</code> and <code>goal</code> consist of lowercase English letters.</li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]"> <template #java>

```java
class Solution {
    public boolean rotateString(String s, String goal) {
        int n = s.length();
        int m = goal.length();
        if (n != m)
            return false;
        return (s + s).contains(goal);
    }
}
```

</template>

<template #cpp>

```cpp
class Solution {
public:
    bool rotateString(string s, string goal) {
        if(s.size() != goal.size()) return false;
        s += s;
        if(s.find(goal) != string :: npos) return true;
        return false;
    }
};
```

</template>

</CodeTabs>
