<div align = "center">
<h style = "margin-bottom: 0px; margin-top: 0px; color : purple;" align = "center" class = "header">

## ⌨ 670. Maximum Swap

</h>
</div>

<h2><a href="https://leetcode.com/problems/maximum-swap" target = "_blank">670. Maximum Swap</a></h2><h3>Medium</h3><hr><p>You are given an integer <code>num</code>. You can swap two digits at most once to get the maximum valued number.</p>

<p>Return <em>the maximum valued number you can get</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> num = 2736
<strong>Output:</strong> 7236
<strong>Explanation:</strong> Swap the number 2 and the number 7.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> num = 9973
<strong>Output:</strong> 9973
<strong>Explanation:</strong> No swap.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= num &lt;= 10<sup>8</sup></code></li>
</ul>

<CodeTabs :languages="[ { name: 'C++', slot: 'cpp' }, { name: 'Java', slot: 'java' } ]"> <template #java>

```java
class Solution {
    public int maximumSwap(int num) {
        char[] digits = Integer.toString(num).toCharArray();
        int[] arr = new int[10];
        for (int i = 0; i < digits.length; i++)
            arr[digits[i] - '0'] = i;
        for (int i = 0; i < digits.length; i++) {
            for (int k = 9; k > digits[i] - '0'; k--) {
                if (arr[k] > i) {
                    char tmp = digits[i];
                    digits[i] = digits[arr[k]];
                    digits[arr[k]] = tmp;
                    return Integer.valueOf(new String(digits));
                }
            }
        }
        return num;
    }
}
```

</template>

<template #cpp>

```cpp
class Solution {
public:
    int maximumSwap(int num) {
        vector<int> digits;
        int secondaryNum = num;
        while(num > 0){
            int temp = num % 10;
            digits.push_back(temp);
            num = num/10;
        }
        int n = digits.size();
        reverse(digits.begin(), digits.end());
        int i = 0;
        bool isSwap = false;
        while(i < n-1){
            int temp = digits[i];
            int maxi = INT_MIN, ind = -1;
            for(int j = i+1; j<n; j++){
                if(digits[j] > temp){
                    if(digits[j] >= maxi){
                        maxi = digits[j];
                        ind = j;
                    }
                }
            }
            if(maxi != INT_MIN){
                swap(digits[i], digits[ind]);
                isSwap = true;
                break;
            }
            else{
                i++;
            }
        }
        if(isSwap == false){
            return secondaryNum;
        }
        int ans = 0;
        for(int k=0; k<n; k++){
            ans = (ans * 10) + digits[k];
        }
        return ans;
    }
};
```

</template>

</CodeTabs>
