# [剑指 Offer 61. 扑克牌中的顺子 - 力扣（LeetCode）](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

## 题目描述

从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

 

示例 1:

输入: [1,2,3,4,5]
输出: True


示例 2:

输入: [0,0,1,2,5]
输出: True


限制：

数组长度为 5 

数组的数取值为 [0, 13] .

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

**排序+模拟**

先将5张牌按从小到大排序，统计赖子的个数`cnt`（即0的个数），并**依次**判断相邻两张牌（其中有牌为大小王就跳过，不用判断）的差值`diff = nums[i] - nums[i - 1]`，至多判断4次。

1. 差值`diff`为0，表示两张牌数字相同，前面已经排除大小王的情况，此种情况不能形成顺子，返回**false**
2. 差值`diff`为1，连续情况，继续判断相邻两张牌。
3. 差值`diff`大于1，此时不连续，需要用赖子来替代缺失值，更新对应`cnt = cnt - (diff - 1)`

最后判断cnt是否不小于0。

```cpp
class Solution {
public:
    bool isStraight(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int cnt = 0;//统计大小王的个数

        for(int i = 0; i < nums.size(); ++i){
            if(nums[i] == 0) {
                ++cnt;
                continue;
            }
            if(i > 0){
                if(nums[i - 1] == 0) continue;
                if(nums[i] == nums[i - 1]) return false;
                cnt = cnt - (nums[i] - nums[i - 1] - 1);
                if(cnt < 0) return false;
            }
        }
        return true;
    }
};
```



这个思路太过细化（复杂），能否简化一点呢？我们站在全局的角度来看：

1. 5张牌如果形成顺子，那最大值-最小值<=4（最大值 与最小值不考虑0）等于4的情况，是5张牌中不含大小王。而小于的情况是出现了赖子。而最大值 与最小值只需要在 $O(n)$ 时间复杂度就能得到。
2. 但是5张牌可能会出现数字相同的情况，查重的话，目前参考题解有三种方式：
   1. 排序判断相邻元素，这个时间复杂度略高
   2. 用set或map来查重，这个空间复杂度略高
   3. 位查重，数字`a`出现，就将对应的二进制置1，当数字`a`再次出现，就会达到查重效果。**此法最佳**，很好的利用数字最大只能是13这一条件。

```cpp
/*
不需要排序
*/
class Solution {
public:
    bool isStraight(vector<int>& nums) {
        int maxNum = -100, minNum = 100, flag = 0;

        for(int i = 0; i < nums.size(); ++i){
            if(nums[i]){
                maxNum = max(maxNum, nums[i]);//最大数
                minNum = min(minNum, nums[i]);//最小数
                if(flag & (1 << (nums[i] - 1))) return false;//二进制位判重
                flag = flag | (1 << nums[i] - 1);//更新二进制位
            }
        }
        return maxNum - minNum <= 4;
    }
};
```

