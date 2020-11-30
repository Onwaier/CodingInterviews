# [剑指 Offer 19. 正则表达式匹配 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/)

## 题目描述

请实现一个函数用来匹配包含'. '和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但与"aa.a"和"ab*a"均不匹配。

示例 1:

输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
示例 2:

输入:
s = "aa"
p = "a*"
输出: true
解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
示例 3:

输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
示例 4:

输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
示例 5:

输入:
s = "mississippi"
p = "mis*is*p*."
输出: false
s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母以及字符 . 和 *，无连续的 '*'。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. 使用regex

```c++
class Solution {
public:
    bool isMatch(string s, string p) {
        regex reg = regex(p);
        return regex_match(s, reg);
    }
};
```

2. dp

dp[i][j]表示目标串的前i个与模式串的前j个能否匹配

1. p[j]是正常字符
直接比较s[i]与p[j]是否相等
dp\[i][j] = dp\[i - 1][j - 1]

2. p[j]是.
dp\[i][j] = dp\[i - 1][j - 1]

3. p[j]是*
要考虑看不看*
如果p[j - 1] != s[i],就不看*
则dp\[i][j] = dp\[i][j - 2]

如果p[j - 1] == s[i]，匹配*
则dp\[i][j] = dp\[i - 1][j]

```c++
/*
动态规划


*/

class Solution {
public:
    bool isMatch(string s, string p) {
        int len1 = s.size();//s的长度
        int len2 = p.size();//p的长度
        vector<vector<int>>dp(len1 + 1, vector<int>(len2 + 1, 0));

        for(int i = 0; i <= len1; ++i){
            for(int j = 0; j <= len2; ++j){
                if(j == 0){//模式串为空
                    dp[i][j] = (i == 0);
                }
                else{//
                    if(j >= 1 && i>= 1 && islower(p[j - 1]) && p[j - 1] == s[i - 1]){//正常字符
                        dp[i][j] = dp[i - 1][j - 1];
                    }
                    else if(j >= 1 && i >= 1 && p[j - 1] == '.'){//p[j - 1]为'.'
                        dp[i][j] = dp[i - 1][j - 1];
                    }
                    else if(j >= 1 && p[j - 1] == '*'){//p[j - 1]为'*'
                        if(j >= 2){//不匹配*
                            dp[i][j] |= dp[i][j - 2];
                        }
                        if(i >= 1 && j >= 2 && (s[i - 1] == p[j - 2] || p[j - 2] == '.')){//匹配*
                            dp[i][j] |= dp[i - 1][j];
                        }
                    }
                }
            }
        }
        return dp[len1][len2];
    }
};
```



  

