<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 433. Minimum Genetic Mutation

</h>
</div>

<h2><a href="https://leetcode.com/problems/minimum-genetic-mutation" target = "_blank">433. Minimum Genetic Mutation</a></h2><h3>Medium</h3><hr><p>A gene string can be represented by an 8-character long string, with choices from <code>&#39;A&#39;</code>, <code>&#39;C&#39;</code>, <code>&#39;G&#39;</code>, and <code>&#39;T&#39;</code>.</p>

<p>Suppose we need to investigate a mutation from a gene string <code>startGene</code> to a gene string <code>endGene</code> where one mutation is defined as one single character changed in the gene string.</p>

<ul>
	<li>For example, <code>&quot;AACCGGTT&quot; --&gt; &quot;AACCGGTA&quot;</code> is one mutation.</li>
</ul>

<p>There is also a gene bank <code>bank</code> that records all the valid gene mutations. A gene must be in <code>bank</code> to make it a valid gene string.</p>

<p>Given the two gene strings <code>startGene</code> and <code>endGene</code> and the gene bank <code>bank</code>, return <em>the minimum number of mutations needed to mutate from </em><code>startGene</code><em> to </em><code>endGene</code>. If there is no such a mutation, return <code>-1</code>.</p>

<p>Note that the starting point is assumed to be valid, so it might not be included in the bank.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> startGene = &quot;AACCGGTT&quot;, endGene = &quot;AACCGGTA&quot;, bank = [&quot;AACCGGTA&quot;]
<strong>Output:</strong> 1
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> startGene = &quot;AACCGGTT&quot;, endGene = &quot;AAACGGTA&quot;, bank = [&quot;AACCGGTA&quot;,&quot;AACCGCTA&quot;,&quot;AAACGGTA&quot;]
<strong>Output:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= bank.length &lt;= 10</code></li>
	<li><code>startGene.length == endGene.length == bank[i].length == 8</code></li>
	<li><code>startGene</code>, <code>endGene</code>, and <code>bank[i]</code> consist of only the characters <code>[&#39;A&#39;, &#39;C&#39;, &#39;G&#39;, &#39;T&#39;]</code>.</li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]">

<template #java>

```java
class Solution {
    static class Pair {
        String s;
        int count;
        public Pair(String s, int count) {
            this.s = s;
            this.count = count;
        }
        @Override
        public String toString() {
            return "(" + s + " " + count + ")";
        }
        @Override
        public boolean equals(Object obj) {
            if (this == obj)
                return true;
            if (obj == null || getClass() != obj.getClass())
                return false;
            Pair current = (Pair)(obj);
            return current.s == s && current.count == count;
        }
        @Override
        public int hashCode() {
            return Objects.hash(s, count);
        }
    }
    public int minMutation(String startGene, String endGene, String[] bank) {
        if (startGene.length() != endGene.length())
            return -1;
        HashSet<String> set = new HashSet<>();
        for (String x : bank)
            set.add(x);
        if (!set.contains(endGene))
            return -1;
        HashSet<String> vis = new HashSet<>();
        vis.add(startGene);
        Queue<Pair> q = new LinkedList<>();
        q.offer(new Pair(startGene, 0));
        int mini = Integer.MAX_VALUE;
        while (q.size() > 0) {
            Pair current = q.poll();
            if (vis.contains(current))
                continue;
            if (current.s.equals(endGene)) {
                mini = Math.min(mini, current.count);
                continue;
            }
            char new_current[] = current.s.toCharArray();
            String temp = "ACGT";
            for (int i = 0; i < current.s.length(); i++) {
                char c = current.s.charAt(i);
                for (int j = 0; j < temp.length(); j++) {
                    new_current[i] = temp.charAt(j);
                    String store = "";
                    for (int x = 0; x < new_current.length; x++)
                        store += new_current[x];
                    if (set.contains(store)) {
                        if (!vis.contains(store))
                            q.offer(new Pair(store, current.count + 1));
                        vis.add(store);
                    }
                    new_current[i] = c;
                }
            }
        }
        if (mini == Integer.MAX_VALUE)
            return -1;
        return mini;
    }
}
```

</template>

<template #cpp>

```cpp
class Solution {
public:
    int minMutation(string startGene, string endGene, vector<string>& bank) {
        int n = bank.size();
        unordered_set<string> bankSet(bank.begin(), bank.end());
        unordered_set<string> visited;
        queue<string> q;
        int steps = 0;
        q.push(startGene);
        visited.insert(startGene);
        while(!q.empty()){
            int size = q.size();
            while(size--){
                auto node = q.front();
                q.pop();
                if(node == endGene) return steps;
                for(int i = 0; i < 8; i++){ 
                    char ogChar = node[i];
                    for(char c : string("ACGT")){ 
                        if(c == ogChar) continue;
                        node[i] = c;
                        if(!visited.count(node) && bankSet.count(node)){
                            q.push(node);
                            visited.insert(node);
                        }
                    }
                    node[i] = ogChar;
                }
            }
            steps++;
        }
        return -1;
    }
};
```

</template>

</CodeTabs>
