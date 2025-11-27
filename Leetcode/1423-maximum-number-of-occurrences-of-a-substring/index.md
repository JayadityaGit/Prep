<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 1423. Maximum Number of Occurrences of a Substring

</h>
</div>

<h2><a href="https://leetcode.com/problems/maximum-number-of-occurrences-of-a-substring" target = "_blank">1423. Maximum Number of Occurrences of a Substring</a></h2><h3>Medium</h3><hr><p>Given a string <code>s</code>, return the maximum number of occurrences of <strong>any</strong> substring under the following rules:</p>

<ul>
	<li>The number of unique characters in the substring must be less than or equal to <code>maxLetters</code>.</li>
	<li>The substring size must be between <code>minSize</code> and <code>maxSize</code> inclusive.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aababcaab&quot;, maxLetters = 2, minSize = 3, maxSize = 4
<strong>Output:</strong> 2
<strong>Explanation:</strong> Substring &quot;aab&quot; has 2 occurrences in the original string.
It satisfies the conditions, 2 unique letters and size 3 (between minSize and maxSize).
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aaaa&quot;, maxLetters = 1, minSize = 3, maxSize = 3
<strong>Output:</strong> 2
<strong>Explanation:</strong> Substring &quot;aaa&quot; occur 2 times in the string. It can overlap.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= maxLetters &lt;= 26</code></li>
	<li><code>1 &lt;= minSize &lt;= maxSize &lt;= min(26, s.length)</code></li>
	<li><code>s</code> consists of only lowercase English letters.</li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]">

<template #java>

```java
class Solution {
    private HashMap<String, Integer> map;
    private int maxi;
    public int maxFreq(String s, int maxLetters, int minSize, int maxSize) {
        int n = s.length();
        map = new HashMap<>();
        maxi = 0;
        for (int i = minSize; i <= maxSize; i++)
            getSubstringsOfLengthK(s, i, maxLetters);
        return maxi;
    }
    private ArrayList<String> getSubstringsOfLengthK(String s, int k, int maxLetters) {
        int n = s.length();
        ArrayList<String> res = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if (i + k <= n) {
                String current = s.substring(i, i + k);
                HashSet<Character> set = new HashSet<>();
                for (int j = 0; j < current.length(); j++)
                    set.add(current.charAt(j));
                if (set.size() <= maxLetters)
                    map.put(current, map.getOrDefault(current, 0) + 1);
                maxi = Math.max(maxi, map.getOrDefault(current, 0));
            }
        }
        return res;
    }
}
```

</template>

<template #cpp>

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
private:
    unordered_map<string, int> freq;
    int maxi;

    void getSubstringsOfLengthK(const string &s, int k, int maxLetters) {
        int n = s.size();
        for (int i = 0; i + k <= n; i++) {
            string sub = s.substr(i, k);
            unordered_set<char> uniqueChars(sub.begin(), sub.end());
            if ((int)uniqueChars.size() <= maxLetters) {
                freq[sub]++;
                maxi = max(maxi, freq[sub]);
            }
        }
    }

public:
    int maxFreq(string s, int maxLetters, int minSize, int maxSize) {
        freq.clear();
        maxi = 0;
        for (int len = minSize; len <= maxSize; len++)
            getSubstringsOfLengthK(s, len, maxLetters);
        return maxi;
    }
};
```

</template>

</CodeTabs>
