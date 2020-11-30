# [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

## 题目描述

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

 

示例：

输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。


提示：

1 <= nums.length <= 50000
1 <= nums[i] <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. 双指针。

   一个从前往后扫描,寻找偶数,一个从后往前扫描,寻找奇数,然后每一对奇数与偶数对换位置,当两指针相遇,表示奇数已全在前面,偶数已全在右边,算法结束。

时间复杂度为：$O(n)$

空间复杂度为：$O(1)$

```cpp
/*
类似快速排序中一趟
时间复杂度为O(n)
空间复杂度为O(1)
*/
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        int idx1 = 0;
        int idx2 = nums.size() - 1;
        while(idx1 < idx2){
            while(idx1 < nums.size() && (nums[idx1] & 1))//从左到右寻找偶数
                ++idx1;
            while(idx2 >= 0 && !(nums[idx2] & 1))
                --idx2;
            if(idx1 < nums.size() && idx2 >= 0 && idx1 < idx2){//从右向左寻找奇数
                swap(nums[idx1], nums[idx2]);//交换奇数与偶数的位置
            }
        }
        return nums;
    }
};
```

