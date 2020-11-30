# [剑指 Offer 53 - II. 0～n-1中缺失的数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

## 题目描述

一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

 

示例 1:

输入: [0,1,3]
输出: 2
示例 2:

输入: [0,1,2,3,4,5,6,7,9]
输出: 8


限制：

1 <= 数组长度 <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

**一次扫描**

$0～n-1$数字有序递增，如果没有数字缺失，正常应该是`nums[i] = i`。

一旦出现数字缺失，便会不满足 ``nums[i] = i``。此时的 **i**即为缺失数字。

一次扫描即可，时间复杂度为：$O(n)$，空间复杂度为：$O(1)$。

```cpp
/*
0-n-1数字有序递增  正常应该是nums[i] = i
若nums[i] = i + 1，则缺失数字为i
时间复杂度为O(n)
空间复杂度为O(1)
*/
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int i = 0, len = nums.size();
        while(i < len && nums[i] == i) ++i;//找到不满足nums[i] = i条件的
        return i;
    }
};
```

**二分查找左边界**

数字有序，很容易想到二分查找
在[left, right]中，

* 当nums[mid] = mid，缺失数字出现在[mid+1, right]
* 不等时，缺失位置的最左侧可能出现在[left, mid] 令right = mid-1 

最后返回left即可。这个过程相当于寻找有序数字中目标出现的左边界。
时间复杂度为：$O(logn)$
空间复杂度为：$O(1)$

```cpp
/*
数字有序，很容易想到二分查找
在[left, right]，当nums[mid] = mid，缺失数字出现在[mid+1, right]
不等时，缺失位置的最左侧可能出现在[left, mid] 令right = mid-1 
最后返回left即可
时间复杂度为：O(logn)
空间复杂度为：O(1)
*/
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int left = 0, right = nums.size() - 1;
        int mid = left + (right - left) / 2;

        while(left <= right){
            mid = left + (right - left) / 2;
            if(nums[mid] == mid){//缺失数字在右侧[mid+1, right]
                left = mid + 1;
            }
            else{//缺失数字左边界在[left, mid]
                right = mid - 1;
            }
        }
        return left;
    }
};
```

