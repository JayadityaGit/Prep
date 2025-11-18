<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 539. Minimum Time Difference

</h>
</div>

<h2><a href="https://leetcode.com/problems/minimum-time-difference" target = "_blank">539. Minimum Time Difference</a></h2><h3>Medium</h3><hr>Given a list of 24-hour clock time points in <strong>&quot;HH:MM&quot;</strong> format, return <em>the minimum <b>minutes</b> difference between any two time-points in the list</em>.
<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<pre><strong>Input:</strong> timePoints = ["23:59","00:00"]
<strong>Output:</strong> 1
</pre><p><strong class="example">Example 2:</strong></p>
<pre><strong>Input:</strong> timePoints = ["00:00","23:59","00:00"]
<strong>Output:</strong> 0
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= timePoints.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>timePoints[i]</code> is in the format <strong>&quot;HH:MM&quot;</strong>.</li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]"> <template #java>

```java
class Solution {
    public int findMinDifference(List<String> timePoints) {
        int n = timePoints.size();
        List<Integer> minutesList = new ArrayList<>();
        for (String time : timePoints) {
            String[] parts = time.split(":");
            int hours = Integer.parseInt(parts[0]);
            int minutes = Integer.parseInt(parts[1]);
            int totalMinutes = hours * 60 + minutes;
            minutesList.add(totalMinutes);
        }
        Collections.sort(minutesList);
        int minDiff = Integer.MAX_VALUE;
        for (int i = 1; i < minutesList.size(); i++) {
            int diff = minutesList.get(i) - minutesList.get(i - 1);
            minDiff = Math.min(minDiff, diff);
        }
        int circularDiff = 1440 - minutesList.get(minutesList.size() - 1) + minutesList.get(0);
        minDiff = Math.min(minDiff, circularDiff);
        return minDiff;
    }
}
```

</template>

<template #cpp>

```cpp
class Solution {
public:
    int findMinDifference(vector<string>& timePoints) {
        int n = timePoints.size();
        vector<int> minutesList;
        minutesList.reserve(n);
        for (const string& time : timePoints) {
            int hours = stoi(time.substr(0, 2));
            int minutes = stoi(time.substr(3, 2));
            int totalMinutes = hours * 60 + minutes;
            minutesList.push_back(totalMinutes);
        }
        sort(minutesList.begin(), minutesList.end());
        int minDiff = INT_MAX;
        for (int i = 1; i < minutesList.size(); i++) {
            int diff = minutesList[i] - minutesList[i - 1];
            minDiff = min(minDiff, diff);
        }
        // Circular difference (last to first across midnight)
        int circularDiff = 1440 - minutesList.back() + minutesList[0];
        minDiff = min(minDiff, circularDiff);
        return minDiff;
    }
};
```

</template>

</CodeTabs>
