# [剑指 Offer 14- II. 剪绳子 II - 力扣（LeetCode）](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)



## 题目描述

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m - 1] 。请问 k[0]*k[1]*...*k[m - 1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 

示例 1：

输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
示例 2:

输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36


提示：

2 <= n <= 1000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

解法同剪绳子I，但题中给的n的范围变大，求幂时，可能会越界，需要手动写快速幂，另一个小问题就是，为避免中间数据越界，数据类型要定义成`long long int`.

```cpp
/*
数学推导
同剪绳子I
注意要用到快速幂以及防止越界数据类型用long long int
*/
class Solution {
public:
    int cuttingRope(int n) {
        if(n <= 3){//长度不足3时， 2最大乘积为1，3最大乘积为2
            return n - 1;
        }
        else{
            const int MOD = 1000000007;
            long long int cnt = 0, res = 0;//绳子分的段数
            cnt = n / 3;
            if(n % 3 == 0){//每段长度均为3
                res = quick_pow(3, cnt);
            }
            else if(n % 3 == 1){
                res = (quick_pow(3, cnt - 1) * 4) % MOD;
            }
            else{
                res = (quick_pow(3, cnt) * 2) % MOD;
            }
            return res;
        }
    }

    long long int quick_pow(int num, int cnt){//快速幂(时间复杂度为：O(logn))
        const int MOD = 1000000007;
        long long int tmp = cnt, res = 1, tmp_pow = num;
        while(tmp){
            if(tmp & 1){
                res = (res * tmp_pow) % MOD;
            }
            tmp = tmp / 2;
            tmp_pow = (tmp_pow * tmp_pow) % MOD;
        }
        return res;
    }
};
```

