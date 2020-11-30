# [剑指 Offer 57 - II. 和为s的连续正数序列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

## 题目描述

输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

 

示例 1：

输入：target = 9
输出：[[2,3,4],[4,5]]
示例 2：

输入：target = 15
输出：[[1,2,3,4,5],[4,5,6],[7,8]]


限制：

1 <= target <= $10^5$

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

**数学求解​**

所有和为target的连续正整数的序列长度都是不一样的。因为当序列长度固定，根据等差数列求和公式可以得到序列的首元素。

如公式所示：a为序列的每一个元素，m为序列的长度。
$$
a + (a + 1) + \dots + (a + m - 1) = target\\
(2a + m - 1)m / 2 = target\\
a = \frac{2 * target - m^2 + m}{2m}
$$
题目要求将序列按长度排列，我们先求出序列长度的上界。那么a应该尽可能的小，即$a = 1$。
$$
1 + 2 + \dots + m = target\\
(1 + m)m / 2 = target\\
m = \frac{-1 + \sqrt{1 + 8 * target}}{2}
$$
求出上界后，只需要遍历序列长度即可，判断每个序列长度下，是否存在连续正整数序列的和为target。

时间复杂度为$O(\sqrt{target})$

空间复杂度为$O(1)$

```cpp
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<vector<int>>res;
        int m = floor((-1 + sqrt(1 + 8 * target)) / 2);//找出长度上界
        while(m >= 2){//遍历序列长度
            int num1 = 2 * target - m * m + m;
            if(num1 % (2 * m) == 0){//存在整数解
                int a = num1 / (2 * m);//序列首元素
                vector<int>tmp;
                for(int i = a; i < a + m; ++i){
                    tmp.push_back(i);
                }
                res.push_back(tmp);
            }
            --m;
        }
        return res;
    }
};
```

**双指针**

参考[题解](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/solution/mian-shi-ti-57-ii-he-wei-sde-lian-xu-zheng-shu-x-2/)

```cpp
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<vector<int>>res;
        int l = 1, r = 2;
        while(l < r){
            int num = (l + r) * (r - l + 1) / 2;
            if(num < target){
                ++r;
            }
            else if(num > target){
                ++l;
            }
            else{
                vector<int>tmp;
                for(int i = l; i <= r; ++i){
                    tmp.push_back(i);   
                }
                res.push_back(tmp);
                ++l;
            }
        }
        return res;
    }
};
```

