<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 1261. Swap For Longest Repeated Character Substring

</h>
</div>

<h2><a href="https://leetcode.com/problems/swap-for-longest-repeated-character-substring" target = "_blank">1261. Swap For Longest Repeated Character Substring</a></h2><h3>Medium</h3><hr><p>You are given a string <code>text</code>. You can swap two of the characters in the <code>text</code>.</p>

<p>Return <em>the length of the longest substring with repeated characters</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> text = &quot;ababa&quot;
<strong>Output:</strong> 3
<strong>Explanation:</strong> We can swap the first &#39;b&#39; with the last &#39;a&#39;, or the last &#39;b&#39; with the first &#39;a&#39;. Then, the longest repeated character substring is &quot;aaa&quot; with length 3.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> text = &quot;aaabaaa&quot;
<strong>Output:</strong> 6
<strong>Explanation:</strong> Swap &#39;b&#39; with the last &#39;a&#39; (or the first &#39;a&#39;), and we get longest repeated character substring &quot;aaaaaa&quot; with length 6.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> text = &quot;aaaaa&quot;
<strong>Output:</strong> 5
<strong>Explanation:</strong> No need to swap, longest repeated character substring is &quot;aaaaa&quot; with length is 5.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= text.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>text</code> consist of lowercase English characters only.</li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]">

<template #java>

```java
class Solution {
    private int pref[][];
    public int maxRepOpt1(String text) {
        int n = text.length();
        pref = new int[n + 1][26];
        for (int i = 0; i < n; i++) {
            int current = text.charAt(i) - 'a';
            pref[i][current]++;
            for (int j = 0; j < 26; j++) {
                if (i - 1 >= 0)
                    pref[i][j] += pref[i - 1][j];
            }
        }
        int low = 1, high = n, ans = 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            System.out.println(mid);
            if (check(text, mid, n)) {
                ans = mid;
                low = mid + 1;
            } else
                high = mid - 1;
        }
        return ans;
    }

    private boolean check(String s, int mid, int n) {
        int freq[] = new int[26];
        for (int i = 0; i < mid; i++)
            freq[s.charAt(i) - 'a']++;
        for (int i = 0; i < 26; i++) {
            if (freq[i] > mid - 1)
                return true;
            if (freq[i] == mid - 1) {
                int total = pref[n - 1][i];
                total -= pref[mid - 1][i];
                if (total > 0)
                    return true;
            }
        }
        int start = 0;
        for (int i = mid; i < s.length(); i++) {
            freq[s.charAt(start++) - 'a']--;
            freq[s.charAt(i) - 'a']++;
            for (int j = 0; j < 26; j++) {
                if (freq[j] == mid - 1) {
                    int total = pref[n - 1][j];
                    int current_total = pref[i][j];
                    if (start - 1 >= 0)
                        current_total -= pref[start - 1][j];
                    if (total - current_total > 0)
                        return true;
                }
            }
        }
        return false;
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
    vector<vector<int>> pref;

    int maxRepOpt1(string text) {
        int n = text.size();
        pref.assign(n + 1, vector<int>(26, 0));

        // Build prefix sum array
        for (int i = 0; i < n; i++) {
            int current = text[i] - 'a';
            pref[i][current]++;
            for (int j = 0; j < 26; j++) {
                if (i - 1 >= 0)
                    pref[i][j] += pref[i - 1][j];
            }
        }

        int low = 1, high = n, ans = 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (check(text, mid, n)) {
                ans = mid;
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return ans;
    }

    bool check(const string &s, int mid, int n) {
        vector<int> freq(26, 0);

        // First window
        for (int i = 0; i < mid; i++)
            freq[s[i] - 'a']++;

        for (int i = 0; i < 26; i++) {
            if (freq[i] > mid - 1)
                return true;

            if (freq[i] == mid - 1) {
                int total = pref[n - 1][i];
                total -= pref[mid - 1][i];
                if (total > 0)
                    return true;
            }
        }
        int start = 0;

        // Sliding window
        for (int i = mid; i < s.size(); i++) {
            freq[s[start] - 'a']--;
            start++;
            freq[s[i] - 'a']++;
            for (int j = 0; j < 26; j++) {
                if (freq[j] == mid - 1) {
                    int total = pref[n - 1][j];
                    int currentTotal = pref[i][j];
                    if (start - 1 >= 0)
                        currentTotal -= pref[start - 1][j];
                    if (total - currentTotal > 0)
                        return true;
                }
            }
        }
        return false;
    }
};
```

</template>

</CodeTabs>
