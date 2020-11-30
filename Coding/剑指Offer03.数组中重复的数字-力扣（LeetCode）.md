# [剑指 Offer 03. 数组中重复的数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/submissions/)



## 题目描述

------

找出数组中重复的数字。

在一出数组中任意一个重复的数字。

**示例 1：**

```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```

**限制：**

```
2 <= n <= 100000
```

来源：力扣（LeetCode
链接：https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题目求解

1. 本题元素大小限制在0-n-1, 可以充分利用这一点，用一个标记数组去标记元素是否出现（这种解法略），但是这样需要另外开辟存储空间，我们可以利用元素本身的非负性，用元素的正负来表示它是否出现过，但这样存在一个问题：0的相反数还是它本身，无法起到标记的作用。我的解决方法是将数组元素整体加1，这样就处于1-n，不会出现元素0了。总共需要两次扫描，时间复杂度为$O(n)$，空间复杂度为$O(1)$。

   

```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        int len = nums.size();//获取数组长度
        for(int i = 0; i < len; ++i){//数组元素整体加1 1-n
            nums[i] += 1;
        }

        for(int i = 0; i < len; ++i){//用自身标记
            if(nums[abs(nums[i]) - 1] < 0){//为负代表出现过
                return abs(nums[i]) - 1;
            }
            nums[abs(nums[i]) - 1] = - nums[abs(nums[i]) - 1];//元素出现过，将其作为下标对应的元素置为负
        }

        return -1;
    }
};
```

2. 下标定位法，参考[题解](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/solution/pythonti-jie-san-chong-fang-fa-by-xiao-xue-66/)，将元素“归位”（元素放到对应的下标处）,若已归位，表示当前元素重复，直接返回。

```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        int len = nums.size();//获取数组长度
        for(int i = 0; i < len; ++i){
            if(nums[i] == i){//元素与下标相同，已“归位”，直接跳过
                continue;
            }
            else{
                int tmp = nums[nums[i]];
                if(tmp == nums[i]){//出现重复元素
                    return nums[i];
                }
                else{//交换元素位置，继续判断交换后的元素是否“归位”
                    nums[nums[i]] = nums[i];
                    nums[i] = tmp;
                    --i;//继续判断当前位置
                }
            }
        }
        return -1;
    }
};
```



