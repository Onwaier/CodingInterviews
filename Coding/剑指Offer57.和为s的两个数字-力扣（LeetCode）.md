# [剑指 Offer 57. 和为s的两个数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

## 题目描述

输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

 

示例 1：

输入：nums = [2,7,11,15], target = 9
输出：[2,7] 或者 [7,2]
示例 2：

输入：nums = [10,26,30,31,47,60], target = 40
输出：[10,30] 或者 [30,10]


限制：

1 <= nums.length <= 10^5
1 <= nums[i] <= 10^6

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

**双指针**

因为数组是递增排序。将两指针$left$与$right$分别指向数组的头与尾。

1. 当$nums[left] + nums[right] < target$，需要增大两数之和，只能将$left$右移，继续1-3步的判断 ；
2. 当$nums[left] + nums[right] > target$，需要减小两数之和，只能将$right$左移，继续1-3步的判断；
3. 相等的话，就找到一个解，直接返回即可。

```cpp
/*
双指针
时间复杂度为：O(n)
空间复杂度为：O(1)
*/
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int>res;
        int left = 0, right = nums.size() - 1;
        while(left < right){
            int sum = nums[left] + nums[right];
            if(sum < target) ++left;
            else if(sum > target) --right;
            else{
                res = vector<int>{nums[left], nums[right]};
                break;
            }
        }
        return res;
    }
};
```

