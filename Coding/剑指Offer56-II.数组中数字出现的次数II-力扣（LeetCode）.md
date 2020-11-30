# [剑指 Offer 56 - II. 数组中数字出现的次数 II - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

## 题目描述

在一个数组 nums 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。

 

示例 1：

输入：nums = [3,4,3,3]
输出：4
示例 2：

输入：nums = [9,1,7,9,7,9,7]
输出：1


限制：

1 <= nums.length <= 10000
1 <= nums[i] < $2^{31}$

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

参考[题解](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/solution/mian-shi-ti-56-ii-shu-zu-zhong-shu-zi-chu-xian-d-4/)

**遍历统计**

相同数字的对应的二进制数也是相同，先不考虑只出现一次的数字，将其余数字的二进制数对应位相加，得到都是3的倍数。

再加入只出现一次的数字，那最后结果只能是 $3k$ 或者 $3k+1$ 。

那么这道题的解题思路就呼之欲出了。

1. 将所有数字的二进制数对应位相加
2. 再将第1步的结果，每位对3取余，结果就是只出现一次的数字的每个二进制位。

时间复杂度为：$O(n)$

空间复杂度为：$O(1)$ 数组为固定长度，常量空间。

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        vector<int>cntArr(31, 0);
        int res = 0;
        //第一位统计1出现的次数
        for(auto num:nums){
            for(int i = 0; i < 31; ++i){
                cntArr[i] += (num & 1);
                num = num >> 1;
            }
        }

        //对每一位1出现的次数对3取余并转化成十进制数
        for(int i = 30; i >= 0; --i){
            res = res * 2 + (cntArr[i] % 3);
        }
        return res;
    }
};
```

**有限状态机**

太复杂，具体参考[题解](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/solution/mian-shi-ti-56-ii-shu-zu-zhong-shu-zi-chu-xian-d-4/)。效率比统计方法高一点。