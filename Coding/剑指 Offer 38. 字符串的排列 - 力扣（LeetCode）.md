# [剑指 Offer 38. 字符串的排列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

## 题目描述

输入一个字符串，打印出该字符串中字符的所有排列。

你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。 

示例:

输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]


限制：

1 <= s 的长度 <= 8



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. next_permutation（有序产生排列）

先对字符串排序，然后用**next_permutation**产生下一个排列，同时把不同的排列存放在set中，利用用**set**去排除重复的。

代码如下：

```cpp
class Solution {
public:
    vector<string> permutation(string s) {
        vector<string>res;
        unordered_set<string>mySet;
        sort(s.begin(), s.end());//排序字符串数组
        res.push_back(s);
        mySet.insert(s);//mySet集合用来剔除重复的排列
        while(next_permutation(s.begin(), s.end())){//用next_permutation产生下一个排列
            if(mySet.find(s) == mySet.end()){
                res.push_back(s);
                mySet.insert(s);
            }  
        }
        return res;
    }
};
```

2. 递归法

```cpp
class Solution {
public:
    vector<string>res;
    string tmp;
    bool mark[10];
    vector<string> permutation(string s) {
        memset(mark, false, sizeof(mark));
        dfs(s, 0, s.size());
        return res;
    }

    void dfs(string &s, int idx, int len){
        if(idx >= len){
            res.push_back(tmp);
            return;
        }
        unordered_set<char>mySet;
        for(int i = 0; i < len; ++i){
            if(!mark[i] && mySet.find(s[i]) == mySet.end()){
                mySet.insert(s[i]);
                tmp.push_back(s[i]);
                mark[i] = true;
                dfs(s, idx + 1, len);
                mark[i] = false;
                tmp.pop_back();
            }
        }
    }
};
```

