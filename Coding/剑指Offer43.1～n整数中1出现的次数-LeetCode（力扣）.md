# [剑指 Offer 43. 1～n 整数中 1 出现的次数 - LeetCode（力扣）](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)

## 题目描述

输入一个整数 n ，求1～n这n个整数的十进制表示中1出现的次数。

例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。

 

示例 1：

输入：n = 12
输出：5
示例 2：

输入：n = 13
输出：6


限制：

$1 <= n < 2^{31}$

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

参考[题解](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/solution/mian-shi-ti-43-1n-zheng-shu-zhong-1-chu-xian-de-2/)

```cpp
/*
时间复杂度为：O(logn)
空间复杂度为：O(1)
*/
class Solution {
public:
    int countDigitOne(int n) {
        long long  digit, low, high, num, res = 0;
        low = 0;
        high = n;
        digit = 1;
        while(high){
            num = high % 10;
            high = high / 10;
                       
            if(num == 0){
                res += high * digit;
            }
            else if(num == 1){
                res += high * digit + low + 1;
            }
            else{
                res += (high + 1) * digit;
            }

            low = num * digit + low;
            digit = digit * 10;
        }
        return res;
    }
};
```

