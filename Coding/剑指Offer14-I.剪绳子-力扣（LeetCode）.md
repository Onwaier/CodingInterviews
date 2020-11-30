# [剑指 Offer 14- I. 剪绳子 - 力扣（LeetCode）](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

## 题目描述

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

示例 1：

输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
示例 2:

输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
提示：

2 <= n <= 58

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/jian-sheng-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

1. 暴力求解



需要考虑所有分段的情况，而每一次分段计算结果所需时间为：$O(1)$

总的时间复杂度为：$O(n)$

```c++
/*
暴力求解
将绳子分成2-n段
每次划分的前m-1段（假设分为m段）相等且尽可能的大，取值为n/m或n/m + 1
*/
class Solution {
public:
    int cuttingRope(int n) {
        int res = -1, tmp = 0, num = 0;
        for(int i = 2; i <= n; ++i){
            num = n / i;
            if(n % i == 0){//能整除的话，直接每段相等
                tmp = pow(num, i);
                tmp_idx = i;
            }
            else{//前i-1段的长度为n/i或者n/i+1
                tmp = max(pow(num, i - 1) * (n - num * (i - 1)), pow(num + 1, i - 1) * (n - (num + 1) * (i - 1)));
                tmp_idx = i;
            }
            if(tmp >= res){//比较找到最大值
                res = tmp;
            }
        }
        return res;
    }
};
```



2. 数学推导



参考[题解](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/solution/mian-shi-ti-14-i-jian-sheng-zi-tan-xin-si-xiang-by/)，将一段绳子分成a段，$n_1, n_2, \cdots, n_{a}$ 。使它们乘积$n_1 n_2 \cdots n_a$最大。利用算术几何均值不等式可知：
$$
\frac{n_{1}+n_{2}+\ldots+n_{a}}{a} \geq \sqrt[a]{n_{1} n_{2} \ldots n_{a}}
$$
当且仅当$n_1 = n_2 = \dots = n_a$时，$n_1 n_2 \cdots n_a$取最大值为$(\frac{n_1 + n_2 + \cdots + n_a}{a})^a = (\frac{n}{a})^a$

利用函数求导发现，当将长度为n的绳子分成$n/e$段，每段长度为e时，乘积最大。但每段绳子的长度只能为整数，经计算发现：
$$
2^{n/2} \leq 3^{n/3}
$$
所以每段的长度应尽量是3，当绳长为n时，将绳子分为$\lceil n/3 \rceil$段，除最后一段的外，其余的都是3，而最后一段的长度可能为1或2。

当其为1时，因为$2 * 2 > 1 * 3$，所以应拿出一段将其划分为2与2。

当其为2时，则无需变更。

时间复杂度为：$O(n)$。

```c++
/*
数学推导
*/
class Solution {
public:
    int cuttingRope(int n) {
        if(n <= 3){
            return n - 1;
        }
        else{
            int res = 0;
            if(n % 3 == 0){//整除，分n/3段，每段长为3
                res = pow(3, n / 3);
            }
            else if(n % 3 == 1){//余1
                res = pow(3, n / 3 - 1) * 2 * 2;
            }
            else{//余2
                res = pow(3, n / 3) * 2;
            }
            return res;
        }
    }
};
```

