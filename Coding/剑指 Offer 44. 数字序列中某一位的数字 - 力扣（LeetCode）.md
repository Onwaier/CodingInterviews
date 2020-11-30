# [剑指 Offer 44. 数字序列中某一位的数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)

## 题目描述

数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从下标0开始计数）是5，第13位是1，第19位是4，等等。

请写一个函数，求任意第n位对应的数字。

 

示例 1：

输入：n = 3
输出：3
示例 2：

输入：n = 11
输出：0


限制：

0 <= n < 2^31

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

思路同[题解](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/solution/mian-shi-ti-44-shu-zi-xu-lie-zhong-mou-yi-wei-de-6/)。

这其实是一道规律题。

| 范围     | 数字个数                 | 位数    | 总位数                             |
| -------- | ------------------------ | ------- | ---------------------------------- |
| 0        | 1                        | 1       | 1                                  |
| 1-9      | 9                        | 1       | 9                                  |
| 10-99    | $90 = 9\times10^{1}$     | 2       | $180=9 \times 10^{1} \times 2$     |
| 100-999  | $900 = 9 \times 10^{2}$  | 3       | $2700 = 9 \times 10^{2} \times 3$  |
| 1000-999 | $9000 = 9 \times 10^{3}$ | 4       | $36000 = 9 \times 10^{3} \times 4$ |
| $\dots$  | $\dots$                  | $\dots$ | $\dots$                            |

首先确定对应的数字处于哪个范围，在该范围的第几个数，这个数的第几位。

代码如下：

```cpp
class Solution {
public:
    int findNthDigit(int n) {
        if(n <= 9){//n不大于9时，直接返回n即可
            return n;
        }
        long long sum = 1, bit = 1;
        long long tmp = 9 * (int)pow(10, bit - 1) * bit;
        while(n >= sum + tmp){
            sum = sum + tmp;
            bit++;//数的位数
            tmp = 9 * (int)pow(10, bit - 1) * bit;
        }

        int idx = (n - sum) / bit;//当前范围中的第几个数
        int idx2 = (n - sum) % bit;//该数的第几位
        return to_string(pow(10, bit - 1) + idx)[idx2] - '0';
    }
};
```

时间复杂度为：$O(logn)$，即与查询的数字的总位数有关

空间复杂度为：$O(logn)$，数字转字符串需要消耗空间。