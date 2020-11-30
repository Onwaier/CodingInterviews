# [剑指 Offer 16. 数值的整数次方 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

## 题目描述

实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

示例 1:

输入: 2.00000, 10
输出: 1024.00000
示例 2:

输入: 2.10000, 3
输出: 9.26100
示例 3:

输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25


说明:

-100.0 < x < 100.0
n 是 32 位有符号整数，其数值范围是 $[−2^{31}, 2^{31} − 1]$ 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

这个题要求手动实现指数幂，朴素的解法是for循环一个一个的乘，时间复杂度为$O(n)$，n是指数的大小。而对此优化的解法是快速幂，时间复杂度为$O(logn)$。

题目中指数给定的范围可能会带来2个问题：

1. 指数为32位无符号整数，相当于c++中int的范围，正负指数所求幂的方式是不同，为了统一，可先将指数取绝对值，然后再根据正负，考虑是否返回其倒数。
2. 按照1这样处理，带来第二个问题是$-2^{31}$的相反数是$2^{31}$超过了int的范围，所以为了避免越界的问题，指数的数据类型不能定义成 **int**。



```c++
/*
快速幂，时间复杂度为：O(logn)
有两个问题：
1. 指数可能为负：指数取绝对值，负数，返回其倒数
2. -2^31的绝对值超过int范围，数据类型定义成long long int
*/
class Solution {
public:
    double myPow(double x, int n) {
        bool flag = true;
        if(n < 0){//标记负指数，结果返回其倒数
            flag = false;
        }
        long long int exp = abs(n);//定义成long long int 防止越界
        double res = 1.0, tmp = x;
        while(exp){
            if(exp & 1){
                res = res * tmp;
            }
            tmp = tmp * tmp;
            exp = exp >> 1;
        }
        if(!flag){//负指数
            return 1/res;
        }
        else{
            return res;
        }
        
    }
};
```

