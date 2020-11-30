# [剑指 Offer 53 - I. 在排序数组中查找数字 I - 力扣（LeetCode）](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

## 题目描述

统计一个数字在排序数组中出现的次数。

 

示例 1:

输入: nums = [5,7,7,8,8,10], target = 8
输出: 2
示例 2:

输入: nums = [5,7,7,8,8,10], target = 6
输出: 0


限制：

0 <= 数组长度 <= 50000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

**二次二分查找**

数组有序，查找某个数字最高效的方法是**二分查找**。这是容易想到的一个思路是：两次利用二分查找，找到target数字出现的左边界和右边界。

首先贴出常用的查找有序数组的左边界与右边界的模板

```cpp
//查找有序数组的左边界模板
int left = 0, right = nums.size() - 1;
while(left <= right){
  mid = left + (right - left) / 2;
  if(nums[mid] < target){
    left = mid + 1;
  }
  else if(nums[mid] >= target){
    right = mid - 1;
  }
}
if(left >= nums.size() || nums[left] != target){//找不到target
  return -1;
}

//查找有序数组的右边界模板
left = 0, right = nums.size() - 1;
while(left <= right){
  mid = left + (right - left) / 2;
  if(nums[mid] <= target){
    left = mid + 1;
  }
  else if(nums[mid] > target){
    right = mid - 1;
  }
}
if(right < 0 || nums[right] != target){//找不到target
  return -1;
}
```



```cpp
/*
二分法找到数字的左边界与右边界
时间复杂度为：O(logn)
*/
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        int mid =  left + (right - left) / 2;
        int boundLeft, boundRight;

        while(left <= right){
            mid = left + (right - left) / 2;
            if(nums[mid] < target){
                left = mid + 1;
            }
            else if(nums[mid] >= target){
                right = mid - 1;
            }
        }
        if(left >= nums.size() || nums[left] != target){//找不到target
            return 0;
        }
        boundLeft = left;
        left = 0, right = nums.size() - 1;
        while(left <= right){
            mid = left + (right - left) / 2;
            if(nums[mid] <= target){
                left = mid + 1;
            }
            else if(nums[mid] > target){
                right = mid - 1;
            }
        }
        boundRight = right;
        return boundRight - boundLeft + 1;
    }
};
```

**优化的二分查找**

参考[题解](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/solution/mian-shi-ti-53-i-zai-pai-xu-shu-zu-zhong-cha-zha-5/)。

优化1：先寻找右边界 **right_bound**，然后寻找左边界时，范围可以从[0, length - 1]缩小到[0, right_bound]。

优化2：为了简化代码。只需要寻找两次右边界, `right_bound(target) - right_bound(target - 1)`，不用单独判断找不到的情形。

```cpp
/*
right_bound(nums, target) - right_bound(nums, target - 1)
时间复杂度为：O(logn)
*/
class Solution {
public:
    int search(vector<int>& nums, int target) {
        return right_bound(nums, target) - right_bound(nums, target - 1);
    }

    int right_bound(vector<int>&nums, int target){
        int left = 0, right = nums.size() - 1, mid;
        while(left <= right){
            mid = left + (right - left) / 2;
            if(nums[mid] <= target){
                left = mid + 1;
            }
            else if(nums[mid] > target){
                right = mid - 1;
            }
        }
        return right;
    }
};
```