# [剑指 Offer 39. 数组中出现次数超过一半的数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

## 题目描述

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 

示例 1:

输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
输出: 2


限制：

1 <= 数组长度 <= 50000



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

一般的思路使用 **map**统计每个元素出现的次数，次数大于一半的即为所示。时间复杂度和空间复杂度均为：$O(n)$。还是一种方式是排序，然后返回中间值。时间复杂度为：$O(nlogn)$，空间复杂度为：$O(1)$。

最好的方法是摩尔投票法，之前写过一篇[从论文角度理解摩尔投票法](https://leetcode-cn.com/problems/majority-element-ii/solution/cong-lun-wen-jiao-du-jiang-jie-mo-er-tou-piao-fa-b/)的文章，具体可以参考这里。

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int cnt = 0, res = nums[0];

        for(int i = 0; i < nums.size(); ++i){
            if(cnt == 0 || nums[i] == res){
                ++cnt;
                res = nums[i];
            }
            else{
                --cnt;
            }
        }

        return res;
    }
};
```

