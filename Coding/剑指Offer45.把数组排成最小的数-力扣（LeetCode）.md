# [剑指 Offer 45. 把数组排成最小的数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

## 题目描述

输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

 

示例 1:

输入: [10,2]
输出: "102"
示例 2:

输入: [3,30,34,5,9]
输出: "3033459"


提示:

0 < nums.length <= 100
说明:

输出结果可能非常大，所以你需要返回一个字符串而不是整数
拼接起来的数字可能会有前导 0，最后结果不需要去掉前导 0

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

先提出解法再证明。

步骤如下：

1. 对于数组中所有数字排照如下规则排序，如果对于任意数字a，b。有`str(a) + str(b) < str(b) + str(a) `，则将数字a排到b前，反之，b在a前。（**`str(a) + str(b)`的含义是将a与b对应的数字字符串拼接起来**）

2. 将排序后的所有数字拼接成字符串即为最后的结果。



证明：

如果拼接的最小的数字对应的数字序列为：$a_{s1}， a_{s2}, \dots ,a_{sn}$。则必有对于任意的i<j，有$str(a_{si}) + str(a_{sj}) \leq str(a_{sj}) + str(a_{ai})$。

下面用**反证法**来证明。

1. $j - i = 1$，即有$str(a_{si}) + str(a_{s,i+1}) > str(a_{s,i+1}) + str(a_{si})$，那么对于数字$a_{s,1},\dots,a_{s,i},a_{s,i+1}，\dots,a_{s,n}$显然存在

$a_{s,1},\dots,a_{s,i+1},a_{s,i},\dots,a_{s,n}$要小于它。矛盾

2. $j - i > 1$，即数字不相邻。可以相邻数字的传递关系来得出矛盾。



代码如下：

```cpp
class Solution {
public:
    string minNumber(vector<int>& nums) {
        sort(nums.begin(), nums.end(), cmp);
        string res = "";
        for(auto num:nums){
            res += to_string(num);
        }
        return res;
    }

    bool static cmp(int a, int b){
        return to_string(a) + to_string(b) < to_string(b) + to_string(a);
    }
};
```

时间复杂度类似快排复杂度，空间复杂度为：$O(logN)$，`to_string`函数要消耗空间。