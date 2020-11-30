# [剑指 Offer 49. 丑数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/chou-shu-lcof/)

## 题目描述

我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

 

示例:

输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
说明:  

1 是丑数。
n 不超过1690。
注意：本题与主站 264 题相同：https://leetcode-cn.com/problems/ugly-number-ii/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/chou-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

**暴力求解**

遍历数字，直到找到第n个丑数为止。

这个时间开销较大，会超时。



```cpp
/*
判别式
判别数是否是丑数 寻找n个丑数
这个时间开销太大，会超时
*/
class Solution {
public:
    int nthUglyNumber(int n) {
        int cnt = 0, res;
        for(int i = 1;;++i){
            if(judge(i)){
                ++cnt;
                res = i;
                if(cnt >= n){
                    break;
                }
            }
        }
        return res;
    }

    bool judge(int num){//判断数是不是丑数
        while(num % 2 == 0){
            num = num / 2;
        }
        while(num % 3 == 0){
            num = num / 3;
        }
        while(num % 5 == 0){
            num = num / 5;
        }
        return num == 1;
    }
};
```

**动态规划**

参考[题解](https://leetcode-cn.com/problems/chou-shu-lcof/solution/chou-shu-ii-qing-xi-de-tui-dao-si-lu-by-mrsate/)

```cpp
/*
生成式
按顺序生成丑数
*/
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int>dp(n, 0);
        int p2, p3, p5;
        p2 = p3 = p5 = 0;
        dp[0] = 1;
        for(int i = 1; i < n; ++i){
            dp[i] = min(dp[p2] * 2, min(dp[p3] * 3, dp[p5] * 5));
            if(dp[i] == dp[p2] * 2) ++p2;
            if(dp[i] == dp[p3] * 3) ++p3;
            if(dp[i] == dp[p5] * 5) ++p5;
        }
        return dp[n - 1];
    }
};
```

