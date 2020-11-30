# [剑指 Offer 15. 二进制中1的个数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)



## 题目描述

请实现一个函数，输入一个整数，输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

示例 1：

输入：00000000000000000000000000001011
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
示例 2：

输入：00000000000000000000000010000000
输出：1
解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
示例 3：

输入：11111111111111111111111111111101
输出：31
解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. 暴力求解

依题意求解即可。

```cpp
/*
暴力求解
时间复杂度为：O(logn)
空间复杂度为：O(1)
*/
class Solution {
public:
    int hammingWeight(uint32_t n) {
        uint32_t tmp = n;
        int cnt = 0;//计数器初始化
        while(tmp){
            if(tmp & 1){//当前二进制位数字为1
                cnt = cnt + 1;//统计二进制位1的个数
            }
            tmp = tmp >> 1;//右移一位
        }
        return cnt;
    }
};
```

2. $n\&(n-1)$

* n-1使得n最右边的一个1变0，其右边的0.

* n&(n-1)中所含的1的个数比原来少1。

* 如此重复，直到最后的n&(n-1)为0为止。

```cpp
/*
暴力求解
时间复杂度为：O(m)
m为1的个数
空间复杂度为：O(1)
*/
class Solution {
public:
    int hammingWeight(uint32_t n) {
        uint32_t tmp = n;
        
        if(tmp == 0){
            return 0;
        }
        int cnt = 1;//计数器初始化
        while(tmp & (tmp - 1)){
            ++cnt;
            tmp = tmp & (tmp - 1);
        }
        return cnt;
    }
};
```

