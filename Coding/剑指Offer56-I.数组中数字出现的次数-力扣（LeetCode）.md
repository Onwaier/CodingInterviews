# [剑指 Offer 56 - I. 数组中数字出现的次数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

## 题目描述

一个整型数组 nums 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。

 

示例 1：

输入：nums = [4,1,4,6]
输出：[1,6] 或 [6,1]
示例 2：

输入：nums = [1,2,10,4,1,4,3,3]
输出：[2,10] 或 [10,2]


限制：

2 <= nums.length <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

参考[题解](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/solution/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-by-leetcode/)

如果只有一个数字只出现一次，那么直接所有数字**异或**即可。

现在有2个数字只出现一次，借鉴**异或**的思路，能否将所有数字分成两组，每组都只含有一个只出现一次的数字呢？

所有数字异或的结果是 **a^b**的结果，将其写成二进制数$x_nx_{n-1}\dots x_1$，当某一位$x_i$等于1，则a与b在这一位上一定是一个是0，而另一个是1，那么我们根据$x_i$来将数字分成两组，就能保证将a与b分到不同的两组，而相同数字分到同一组。

对两组数字在分别异或，即可求出结果。

```cpp
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        int res = 0;
        //1. 所有数字异或
        for(auto num:nums){
            res = res ^ num;
        }

        //2.寻找任意位为1的
        int cnt = 0, tmp = 1;
        while(!(res & tmp)) {
            ++cnt;
            tmp = tmp << 1;
        }

        //根据第cnt位来分组 为0的一组，为1的一组
        int group1 = 0, group2 = 0;
        for(auto num:nums){
            if(num & tmp){
                group1 = group1 ^ num;
            }
            else{
                group2 = group2 ^ num;
            }
        }

        return vector<int>{group1, group2};
    }
};
```

