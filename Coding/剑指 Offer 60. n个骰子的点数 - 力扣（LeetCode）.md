# [剑指 Offer 60. n个骰子的点数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)

## 题目描述

把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。

 

你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。

 

示例 1:

输入: 1
输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]
示例 2:

输入: 2
输出: [0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778]


限制：

1 <= n <= 11



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

**递归法**

**会超时**！！！用递归找到点数之和为s时的可能情况个数。

```cpp
/*
重要的是得到s的每个取值出现的可能组合数
*/
class Solution {
public:
    int sum, cnt;
    vector<double> dicesProbability(int n) {
        sum = 0, cnt = 0;
        vector<double>res;
        double num = pow(6, n);
        for(int i = n; i <= n * 6; ++i){
            cnt = sum = 0;//初始化
            getCnt(n, i, 0);
            res.push_back(cnt / num);//加入结果集
        }
        return res;
    }

    void getCnt(int n, int s, int idx){
        if(idx >= n - 1){//递归出口
            if(s - sum <= 6){//最后一个的点数不能超过6
                ++cnt;
            }
            return;
        }
        for(int i = 1; i <= 6; ++i){
            if(sum + i >= s){//当前和超过s,表示不可能
                return;
            }
            sum += i;
            getCnt(n, s, idx + 1);//继续递归
            sum -= i;//恢复环境
        }
    }
};
```

**动态规划**

如果抛n个骰子点数为s的情况可能数记为：$dp[n][s]$，则它可以看成

$dp[n-1][s-1] + dp[n-1][s-2]+dp[n-1][s-3]+dp[n-1][s-4]+dp[n-1][s-5]+dp[n-1][s-6]$

那求$d[n][s]$转化成了求$dp[n-1][s-1, s-2, \dots, s-6]$。而可以进一步转化，最后都可以用$dp[1][1, 2, \dots, 6]$表示。

我们采用由底向上的思路，初始化$dp[1][1] = dp[1][2] = \dots = dp[1][6] = 1$。

```cpp
/*
重要的是得到s的每个取值出现的可能组合数
*/
class Solution {
public:
    vector<double> dicesProbability(int n) {
       vector<vector<int>>dp(n + 1, vector<int>(n * 6 + 1, 0));
       vector<double>res;
       double num = pow(6, n);
       
       //初始化
       for(int i = 1; i <= 6; i++){
           dp[1][i] = 1;
       }

      	//自底向上依次求dp[t][t, ..., t * 6](t = 2, ..., n)
        for(int i = 2; i <= n; ++i){
            for(int s = i; s <= i * 6; ++s){
                int sum = 0;
                for(int t = 1; t <= 6; ++t){
                    if(s - t >= i - 1 ) sum += dp[i - 1][s - t];
                    else break;
                }
                dp[i][s] = sum;
            }
        }

      	//最后将dp[n][n, ..., n * 6]统一除以6^n
        for(int s = n; s <= n * 6; ++s){
            res.push_back(dp[n][s] / num);
        }

        return res;
    }

   	
};
```

**动态规划+空间优化**

因为我们最后需要的仅仅是结果$dp[n][n, \dots, n * 6]$，不需要前面的存储结果。那我们用一维数组来$dp[1, 2, \dots, n * 6]$来表示吗？

$dp[n][s]$的计算依赖于$dp[n-1][s-1, \dots, s-6]$，按顺序计算$dp[n][n], dp[n][n+1], \dots$，会在计算完$dp[n][n]$时将$dp[n-1][n]$覆盖掉，这不是我们所希望的，为了避免这种情况，我们可以倒着计算，即按照$dp[n][6*n], dp[n][6*n-1],\dots$的顺序来计算。这样就可以保证我们需要的数据不会被覆盖掉了。从而实现了用一维数组来存储最后结果。

```cpp
/*
重要的是得到s的每个取值出现的可能组合数
*/
class Solution {
public:
    vector<double> dicesProbability(int n) {
        vector<int>dp(6 * n + 1, 0);
       vector<double>res;
       double num = pow(6, n);
       
       //初始化
       for(int i = 1; i <= 6; i++){
           dp[i] = 1;
       }

        for(int i = 2; i <= n; ++i){
            for(int s = i * 6; s >= i; --s){//倒着算
                int sum = 0;
                for(int t = 1; t <= 6; ++t){
                    if(s - t >= i - 1) sum += dp[s - t];
                    else break;
                }
                dp[s] = sum;
            }
        }

        for(int s = n; s <= n * 6; ++s){
            res.push_back(dp[s] / num);
        }

        return res;
    }

   
};
```

