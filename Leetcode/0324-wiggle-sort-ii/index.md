<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 324. Wiggle Sort II

</h>
</div>

<h2><a href="https://leetcode.com/problems/wiggle-sort-ii/description/?envType=problem-list-v2&envId=sorting" target = "_blank">324. Wiggle Sort II</a></h2><h3>Medium</h3><hr><p>Given an integer array <code>nums</code>, reorder it such that <code>nums[0] &lt; nums[1] &gt; nums[2] &lt; nums[3]...</code>.</p>

<p>You may assume the input array always has a valid answer.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,5,1,1,6,4]
<strong>Output:</strong> [1,6,1,5,1,4]
<strong>Explanation:</strong> [1,4,1,5,1,6] is also accepted.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,3,2,2,3,1]
<strong>Output:</strong> [2,3,1,3,1,2]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 5000</code></li>
	<li>It is guaranteed that there will be an answer for the given input <code>nums</code>.</li>
</ul>

<p>&nbsp;</p>
<strong>Follow Up:</strong> Can you do it in <code>O(n)</code> time and/or <strong>in-place</strong> with <code>O(1)</code> extra space?

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' },
  { name: 'Java', slot: 'java' }
]">

<template #java>

```java
class Solution {
    static class custom_sort implements Comparator<Integer> {
        @Override
        public int compare(Integer first, Integer second) {
            return Integer.compare(second, first);
        }
    }
    public void wiggleSort(int[] nums) {
        int n = nums.length;
        PriorityQueue<Integer> pq = new PriorityQueue<>(new custom_sort());
        for (int ele : nums)
            pq.offer(ele);
        int ind = 1;
        while (ind < n) {
            nums[ind] = pq.poll();
            ind += 2;
        }
        ind = 0;
        while (pq.size() > 0) {
            nums[ind] = pq.poll();
            ind += 2;
        }
    }
}
```

</template>

<template #cpp>

```cpp
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        int n = nums.size();
        priority_queue<int> pq;
        for(auto &i : nums){
            pq.push(i);
        }
        int i = 1;
        while(i < n && !pq.empty()){
            nums[i] = pq.top();
            pq.pop();
            i += 2;
        }
        int j = 0;
        while(j < n && !pq.empty()){
            nums[j] = pq.top();
            pq.pop();
            j += 2;
        }
    }
};
```

</template>

</CodeTabs>
