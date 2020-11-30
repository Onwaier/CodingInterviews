# [剑指 Offer 46. 把数字翻译成字符串 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

## 题目描述

给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

 

示例 1:

输入: 12258
输出: 5
解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"


提示：

$0 <= num < 2^{31}$

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

![image-20201030123149892](/Users/onwaier/Library/Application%20Support/typora-user-images/image-20201030123149892.png)

从图中可以看出$12258的翻译个数 = 2258的翻译个数+ 258的翻译个数$，而2258与258可以继续分解，直到只剩一位数为止。

这可以用递归和动态规划来做。



**动态规划**

数字**num**对应的字符串为**str**，**dp[i]**表示从第i位开始到结尾的数字的翻译次数。

**如果str[i]=='0'或者stoi(str.substr(i, 2))>25，则dp[i] = dp[i + 1]**

**否则dp[i] = dp[i + 1] + dp[i + 2]**



时间复杂度为：$O(logn)$

空间复杂度为：O(logn)

代码如下：

```cpp
/*
动态规划
dp[i]表示从第i位到结尾处的数字翻译方法次数
*/
class Solution {
public:
    int translateNum(int num) {
        string str = to_string(num);//num转string
        int len = str.size();
        vector<int>dp(len + 1, 1);

        for(int i = len - 2; i >= 0; --i){
            if(stoi(str.substr(i, 2)) > 25 || str[i] == '0'){
                dp[i] = dp[i + 1];
            }
            else{
                dp[i] = dp[i + 1] + dp[i + 2];
            }
        }

        return dp[0];
    }
};
```

