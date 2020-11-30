# [剑指 Offer 48. 最长不含重复字符的子字符串 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

## 题目描述

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

 

示例 1:

输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。



示例 2:

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。



示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。



提示：

s.length <= 40000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

动态规划

参考**连续子数组最大和**，第一反应是采用动态规划。定义$dp[i]$为以**s[i]**结尾的最长子字符串的长度。但在计算 $dp[i+1]$的时候发现，要知道与 **s[i]**相同字符的最近下标和$dp[i]$对应的最长字符串的起始下标（start）。

状态转换方程为：
$$
\begin{equation}
dp[i] = 
\begin{cases}
i - mark[s[i]]&s[i]在前面出现过且mark[s[i]] \geq start\\
dp[i - 1]+1 & others
\end{cases}
\end{equation}
$$


```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int len = s.size();
        if(len == 0){
            return 0;
        }

        vector<int>dp(len, 1);
        int res = 1, start = 0;;
        map<char, int>mark;
        mark[s[0]] = 0;

        for(int i = 1; i < len; ++i){
            
            if(mark.find(s[i]) != mark.end() && mark[s[i]] >= start){
                dp[i] = i - mark[s[i]];
                start = mark[s[i]] + 1;
            }
            else{
                dp[i] = dp[i - 1] + 1;
            }
            // printf("%d %d\n", start, dp[i]);
            res = max(res, dp[i]);
            mark[s[i]] = i;
        }

        return res;
    }
    
};
```

滑动窗口

发现重复字符时，滑动窗口。直到窗口内没有重复的字符为止。

时间复杂度为：$O(n)$

空间复杂度为：$O(n)$

```cpp
/*
滑动窗口 时间复杂度为：O(n)
空间复杂度为：O(n)
*/
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int start = 0;
        int len = s.size(), res = 1;
        map<char, int>mark;
        if(0 == len){
            return 0;
        }
        for(int i = 0; i < len; ++i){
            mark[s[i]]++;
            if(mark[s[i]] > 1){//s[i]字符重复 滑动窗口找到重复字符
                while(s[start] != s[i]){
                    mark[s[start]]--;
                    ++start;
                }
                mark[s[start]]--;
                ++start;
            }
            res = max(res, i - start + 1);
        }
        return res;
    }
    
};
```

