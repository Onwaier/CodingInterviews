# 剑指 Offer 05. 替换空格 - 力扣（LeetCode）

## 题目描述

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

 

示例 1：

输入：s = "We are happy."
输出："We%20are%20happy."


限制：

0 <= s 的长度 <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

1. 直接一次扫描，遇到空格替换成%20.



```c++
/*
一次扫描，遇到非空格直接添加；遇到空格替换成%20
时间复杂度为：O(n)，空间复杂度为：O(n)
*/
class Solution {
public:
    string replaceSpace(string s) {
        string res = "";
        int len = s.size();
        for(int i = 0; i < len; ++i){
            if(s[i] != ' '){//不是空格直接添加到res
                res.push_back(s[i]);
            }
            else{//遇到空格添加一个“%20”的字符串
                res.append("%20");
            }
        }
        return res;
    }
};
```



2. 