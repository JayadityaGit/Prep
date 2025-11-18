<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 677. Map Sum Pairs

</h>
</div>

<h2><a href="https://leetcode.com/problems/map-sum-pairs" target = "_blank">677. Map Sum Pairs</a></h2><h3>Medium</h3><hr><p>Design a map that allows you to do the following:</p>

<ul>
	<li>Maps a string key to a given value.</li>
	<li>Returns the sum of the values that have a key with a prefix equal to a given string.</li>
</ul>

<p>Implement the <code>MapSum</code> class:</p>

<ul>
	<li><code>MapSum()</code> Initializes the <code>MapSum</code> object.</li>
	<li><code>void insert(String key, int val)</code> Inserts the <code>key-val</code> pair into the map. If the <code>key</code> already existed, the original <code>key-value</code> pair will be overridden to the new one.</li>
	<li><code>int sum(string prefix)</code> Returns the sum of all the pairs&#39; value whose <code>key</code> starts with the <code>prefix</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input</strong>
[&quot;MapSum&quot;, &quot;insert&quot;, &quot;sum&quot;, &quot;insert&quot;, &quot;sum&quot;]
[[], [&quot;apple&quot;, 3], [&quot;ap&quot;], [&quot;app&quot;, 2], [&quot;ap&quot;]]
<strong>Output</strong>
[null, null, 3, null, 5]

<strong>Explanation</strong>
MapSum mapSum = new MapSum();
mapSum.insert(&quot;apple&quot;, 3);  
mapSum.sum(&quot;ap&quot;);           // return 3 (<u>ap</u>ple = 3)
mapSum.insert(&quot;app&quot;, 2);    
mapSum.sum(&quot;ap&quot;);           // return 5 (<u>ap</u>ple + <u>ap</u>p = 3 + 2 = 5)
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= key.length, prefix.length &lt;= 50</code></li>
	<li><code>key</code> and <code>prefix</code> consist of only lowercase English letters.</li>
	<li><code>1 &lt;= val &lt;= 1000</code></li>
	<li>At most <code>50</code> calls will be made to <code>insert</code> and <code>sum</code>.</li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]"> <template #java>

```java
class MapSum {
    private HashMap<String, Integer> map;
    public MapSum() {
        map = new HashMap<>();
    }
    public void insert(String key, int val) {
        map.put(key, val);
    }
    public int sum(String prefix) {
        int Total_Sum = 0;
        for (Map.Entry<String, Integer> curr : map.entrySet()) {
            String key = curr.getKey();
            if (key.startsWith(prefix))
                Total_Sum += curr.getValue();
        }
        return Total_Sum;
    }
}
/**
    Your MapSum object will be instantiated and called as such:
    MapSum obj = new MapSum();
    obj.insert(key,val);
    int param_2 = obj.sum(prefix);
*/
```

</template>

<template #cpp>

```cpp
class MapSum {
public:
    unordered_map<string, int> mp;
    MapSum() {
        // nothing needed, mp initializes itself
    }
    void insert(string key, int val) {
        mp[key] = val;
    }
    int sum(string prefix) {
        int total = 0;
        for (const auto& it : mp) {
            const string& key = it.first;
            if (key.rfind(prefix, 0) == 0) {
                // rfind(prefix, 0) == 0 → key.startsWith(prefix)
                total += it.second;
            }
        }
        return total;
    }
};
```

</template>

</CodeTabs>
