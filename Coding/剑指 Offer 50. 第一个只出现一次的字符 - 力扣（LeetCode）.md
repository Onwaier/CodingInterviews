# [剑指 Offer 50. 第一个只出现一次的字符 - 力扣（LeetCode）](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

## 题目描述

在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

示例:

s = "abaccdeff"
返回 "b"



s = "" 
返回 " "


限制：

0 <= s 的长度 <= 50000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

**统计小写字母个数**

第一次扫描 用数组统计所有小写字母出现次数

第二次扫描 返回第一个只出现一次的字符

时间复杂度为：$O(n)$

空间复杂度为：$O(1)$， 因为字符串只可能出现小写字母，而小写字母的个数是26个 固定的，所以只需要常量空间。

```cpp
/*
第一次扫描 用数组统计所有小写字母出现次数
第二次扫描 返回第一个只出现一次的字符
时间复杂度为：O(n)
空间复杂度为：O(1)
*/
class Solution {
public:
    char firstUniqChar(string s) {
        int cnt[26] = {0};

        for(auto ch:s){
            cnt[ch - 'a']++;
        }

        for(auto ch:s){
            if(cnt[ch - 'a'] == 1){
                return ch;
            }
        }
        return ' ';
    }
};
```



**优化**

参考[题解](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/solution/mian-shi-ti-50-di-yi-ge-zhi-chu-xian-yi-ci-de-zi-3/)，可以用 **map** 或者 **unordered_map** 去统计每个小写字母的个数。另一个小优化时，用vector存放在字符串出现的字符，这样第二次直接扫描vector数组，最差也只需要26次就能找到只出现一次的字符，效率高了很多。