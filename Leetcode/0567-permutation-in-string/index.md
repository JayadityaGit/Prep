<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 567. Permutation in String

</h>
</div>

<h2><a href="https://leetcode.com/problems/permutation-in-string" target = "_blank">567. Permutation in String</a></h2><h3>Medium</h3><hr><p>Given two strings <code>s1</code> and <code>s2</code>, return <code>true</code> if <code>s2</code> contains a <span data-keyword="permutation-string">permutation</span> of <code>s1</code>, or <code>false</code> otherwise.</p>

<p>In other words, return <code>true</code> if one of <code>s1</code>&#39;s permutations is the substring of <code>s2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s1 = &quot;ab&quot;, s2 = &quot;eidbaooo&quot;
<strong>Output:</strong> true
<strong>Explanation:</strong> s2 contains one permutation of s1 (&quot;ba&quot;).
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s1 = &quot;ab&quot;, s2 = &quot;eidboaoo&quot;
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s1.length, s2.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s1</code> and <code>s2</code> consist of lowercase English letters.</li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]"> <template #java>

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int n1 = s1.length(), n2 = s2.length();
        if (n1 > n2)
            return false;
        int[] arr1 = new int[26];
        int[] arr2 = new int[26];
        for (char ch : s1.toCharArray())
            arr1[ch - 'a']++;
        for (int i = 0; i < n2; i++) {
            arr2[s2.charAt(i) - 'a']++;
            if (i >= n1)
                arr2[s2.charAt(i - n1) - 'a']--;
            if (Arrays.equals(arr1, arr2))
                return true;
        }
        return false;
    }
}
```

</template>

<template #cpp>

```cpp
class Solution {
public:
    bool check(vector<int> &a , vector<int> &b){
        for(int i = 0 ; i < a.size() ; i++){
            if(a[i] != b[i]) return false;
        }
        return true;
    }
    bool checkInclusion(string s1, string s2) {
        if(s2.size() < s1.size()) return false;
        vector<int> freq(26 , 0);
        for(auto it : s1){
            freq[it - 'a']++;
        }
        vector<int> freq2(26 , 0);
        int i = 0;
        int j = 0;
        while(j < s2.size()){
            freq2[s2[j] - 'a']++;
            if(j - i + 1 == s1.size()){
                bool flag = check(freq, freq2);
                if(flag) return true;
            }
            if(j - i + 1 < s1.size()){
                j++;
            }
            else{
                freq2[s2[i] - 'a']--;
                i++;
                j++;
            }
        }
        return false;
    }
};
```

</template>

</CodeTabs>
