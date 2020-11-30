# [剑指 Offer 59 - I. 滑动窗口的最大值 - 力扣（LeetCode）](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

## 题目描述

给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

示例:

输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7


提示：

你可以假设 k 总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

参考[题解](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/solution/)

维持一个队列，存放当前窗口元素（非严格递减）。当新增元素大于队尾元素，就出队尾，直到队尾元素不大于它或者队列为空。

那当前窗口的最大值就是队列中队首元素，每次窗口移动时，要判断中窗口中移除的元素与队列的队首元素是否相同，再选择是否出队首元素。

```cpp
/*
单调队列(存储非严格递减序列)
*/
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int>res;
        deque<int>que;
        int len = nums.size();
        if(0 == len){
            return res;
        }
        //初始化窗口
        for(int i = 0; i < k; ++i){
            while(que.size() && que.back() < nums[i]){
                que.pop_back();
            }
            que.push_back(nums[i]);
        }
        res.push_back(que.front());
        for(int i = k; i < len; ++i){
            //更新队列
            if(que.front() == nums[i - k]){
                que.pop_front();
            }

            //更新窗口
            while(que.size() && que.back() < nums[i]){
                que.pop_back();
            }
            que.push_back(nums[i]);
            //取出最大值
            res.push_back(que.front());
        }
        return res;
    }
};
```

