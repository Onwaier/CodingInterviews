# [剑指 Offer 10- I. 斐波那契数列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)



## 题目描述

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 

示例 1：

输入：n = 2
输出：1
示例 2：

输入：n = 5
输出：5


提示：

0 <= n <= 100

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. 递推式求斐波那契数列

时间复杂度为：$O(n)$

空间复杂度为：$O(1)$

```c++
/*
利用递推式求第n个斐波那契数
时间复杂度为：O(n)
空间复杂度为：O(1)
*/
class Solution {
public:
    int fib(int n) {
        if(n == 0){//第1项为0
            return 0;
        }
        else if(n == 1){//第2项为1
            return 1;
        }
        else{
            int a = 0, b = 1, res;
            const int MOD = 1000000007;
            for(int i = 2; i <= n; ++i){//递推求解第n项
                res = (a + b) % MOD ;//记得取模
                a = b;
                b = res;
            }
            return res;
        }
    }
};
```



2. 矩阵幂

将斐波那契数列用公式表示如下
$$
arr[1] = 0\\
arr[2] = 1\\

\begin{pmatrix}
 arr[2] & arr[3]
\end{pmatrix}=
\begin{pmatrix}
 arr[1] & arr[2]
\end{pmatrix}
\begin{pmatrix}
 0 & 1\\ 1 & 1
\end{pmatrix}\\

\begin{pmatrix}
 arr[3] & arr[4]
\end{pmatrix}=
\begin{pmatrix}
 arr[2] & arr[3]
\end{pmatrix}
\begin{pmatrix}
 0 & 1\\ 1 & 1
\end{pmatrix}=
\begin{pmatrix}
 arr[1] & arr[2]
\end{pmatrix}
\begin{pmatrix}
 0 & 1\\ 1 & 1
\end{pmatrix}^2\\

\cdots\\

\begin{pmatrix}
 arr[n - 1] & arr[n]
\end{pmatrix}=
\begin{pmatrix}
 arr[n - 2] & arr[n - 1]
\end{pmatrix}
\begin{pmatrix}
 0 & 1\\ 1 & 1
\end{pmatrix}=
\begin{pmatrix}
 arr[1] & arr[2]
\end{pmatrix}
\begin{pmatrix}
 0 & 1\\ 1 & 1
\end{pmatrix}^{n - 2}
$$
可以将斐波拉契数的求解转换成矩阵幂问题。

而用快速幂的方法可以在$O(logn)$时间内求解问题.





