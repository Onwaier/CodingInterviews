# [剑指 Offer 42. 连续子数组的最大和 - 力扣（LeetCode）](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

## 题目描述

输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

 

示例1:

输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。


提示：

1 <= arr.length <= 10^5
-100 <= arr[i] <= 100

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. 动态规划

很经典的一道动态规划题目，本题定义的状态`dp[i]`表示以`nums[i]`结尾的连续序列的最大值，存在明显的最小子结构。状态转换方程为：`dp[i] = max(dp[i], dp[i - 1] + nums[i])`。

时间复杂度为：$O(n)$，因为仅需扫描一遍**nums**数组。

空间复杂度为：$O(n)$，因为需要定义`dp`数组来存储状态，长度与`nums`数组相同。

```cpp
/*
动态规划
时间复杂度为：O(n)
空间复杂度为：O(n)
dp[i]表示以nums[i]结尾的连续序列的最大值
*/
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int len = nums.size(), res = INT_MIN;
        vector<int>dp(len, 0);
        for(int i = 0; i < len; ++i){
            if(i == 0){
                dp[i] = nums[i];
            }
            else{
                dp[i] = max(nums[i], dp[i - 1] + nums[i]);
            }
            res = max(res, dp[i]);
        }
        return res;
    }
};
```

因为`dp[i]`只与`nums[i]`和`dp[i - 1]`有关，所以我只需要pre和now变量去存储dp[i-1]与dp[i]即可，而不需要使用dp数组，空间复杂度优化为$O(1)$。

```cpp
/*
动态规划
时间复杂度为：O(n)
空间复杂度为：O(1)
now表示以nums[i]结尾的连续序列的最大值
pre表示以nums[i-1]结尾的连续序列的最大值
*/
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int len = nums.size(), res = INT_MIN;
        int pre = nums[0], now = nums[0];
        for(int i = 0; i < len; ++i){
            pre = now;
            if(i == 0){
                now = nums[i];
            }
            else{
                now = max(nums[i], pre + nums[i]);
            }
            res = max(res, now);
        }
        return res;
    }
};
```



2. 分治法

连续子段和的最大值可能出现在三种情况之一：左半段，右半段，跨中间那段。不断二分，直到区间中只有一个元素为止。时间复杂度为：$O(nlogn)$（并不符合题意要求，动态规划是这道题最好的解法），空间复杂度为：$O(logn)$。

```cpp
/*
分治
时间复杂度为：O(nlogn)
空间复杂度为：O(logn)
*/
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        return solve(nums, 0, nums.size() - 1);
    }
    int solve(vector<int>& nums, int l, int r){
        if(l == r){
            return nums[l];
        }
        else{//二分法求解
            int mid = l + (r - l) / 2;
            int lMaxSum = solve(nums, l, mid);
            int rMaxSum = solve(nums, mid + 1, r);
            int midMaxSum = getMidMaxSum(nums, l, r, mid);
            return max(midMaxSum, max(lMaxSum, rMaxSum));
        }
    }

    int getMidMaxSum(vector<int>& nums, int l, int r, int mid){//用贪心法对中间段求和
        int lMaxSum = nums[mid], rMaxSum = nums[mid + 1];//求以nums[mid]结尾的最大连续和
        int lTmpSum = nums[mid], rTmpSum = nums[mid + 1];//求以nums[mid + 1]开始的最大连续和
        for(int i = mid - 1; i >= l; --i){
            lTmpSum += nums[i];
            lMaxSum = max(lMaxSum, lTmpSum);
        }
        for(int i = mid + 2; i <= r; ++i){
            rTmpSum += nums[i];
            rMaxSum = max(rMaxSum, rTmpSum);
        }
        return lMaxSum + rMaxSum;
    }
};
```

