# [剑指 Offer 58 - I. 翻转单词顺序 - 力扣（LeetCode）](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

## 题目描述

输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

 

示例 1：

输入: "the sky is blue"
输出: "blue is sky the"
示例 2：

输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
示例 3：

输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。


说明：

无空格字符构成一个单词。
输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

主要思路是：

1. 把字符串按空格分隔成每个单词
2. 把单词按逆序用空格连起来

```cpp
/*
主要思路就是把字符串按空格分隔成每个单词
然后把单词按逆序用空格连起来
*/
class Solution {
public:
    string reverseWords(string s) {
        vector<string>res;
        int idx = 0, idx1 = 0, len = s.size();
        string tmp;

        //分隔字符串
        while(idx < len && s[idx] == ' ') ++idx;//清除字符串前面的空格
        while((idx1 = s.find(' ', idx)) != -1){
            res.push_back(s.substr(idx, idx1 - idx));
            idx = idx1;
            while(idx < len && s[idx] == ' ') ++idx;//清除字符串中间的空格
        }
        if(idx < len) res.push_back(s.substr(idx));//处理最后一个word
        
        for(int i = res.size() - 1; i >= 0; --i){//连接单词
            i == res.size() - 1?tmp.append(res[i]):tmp.append(" " + res[i]);
        }
        return tmp;
    }
};
```

