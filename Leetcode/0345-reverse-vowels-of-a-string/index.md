<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 345. Reverse Vowels of a String

</h>
</div>

<h2><a href="https://leetcode.com/problems/reverse-vowels-of-a-string" target = "_blank">345. Reverse Vowels of a String</a></h2><h3>Easy</h3><hr><p>Given a string <code>s</code>, reverse only all the vowels in the string and return it.</p>

<p>The vowels are <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code>, and <code>&#39;u&#39;</code>, and they can appear in both lower and upper cases, more than once.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;IceCreAm&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">&quot;AceCreIm&quot;</span></p>

<p><strong>Explanation:</strong></p>

<p>The vowels in <code>s</code> are <code>[&#39;I&#39;, &#39;e&#39;, &#39;e&#39;, &#39;A&#39;]</code>. On reversing the vowels, s becomes <code>&quot;AceCreIm&quot;</code>.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;leetcode&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">&quot;leotcede&quot;</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 3 * 10<sup>5</sup></code></li>
	<li><code>s</code> consist of <strong>printable ASCII</strong> characters.</li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' },
  { name: 'Java', slot: 'java' }
]">

<template #java>

```java
class Solution {
    public String reverseVowels(String s) {
        int n = s.length();
        ArrayList<Integer> res = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            char current = s.charAt(i);
            if (isVowel(current))
                res.add(i);
        }
        Collections.reverse(res);
        int k = 0;
        String ans = "";
        for (int i = 0; i < n; i++) {
            char current = s.charAt(i);
            if (isVowel(current))
                ans += s.charAt(res.get(k++));
            else
                ans += current;
        }
        return ans;
    }
    private boolean isVowel(char current) {
        if (current == 'a' || current == 'e' || current == 'i' || current == 'o' || current == 'u')
            return true;
        if (current == 'A' || current == 'E' || current == 'I' || current == 'O' || current == 'U')
            return true;
        return false;
    }
}
```

</template>

<template #cpp>

```cpp
class Solution {
public:
    bool isVowel(char ch) {
        ch = tolower(ch); 
        return (ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u');
    }
    string reverseVowels(string s) {
        string temp;
        for (char ch : s) {
            if (isVowel(ch)) temp += ch;
        }
        reverse(temp.begin(), temp.end());
        int cnt = 0;
        for (int i = 0; i < s.size(); i++) {
            if (isVowel(s[i])) {
                s[i] = temp[cnt];
                cnt++;
            }
        }
        return s;
    }
};
```

</template>

</CodeTabs>
