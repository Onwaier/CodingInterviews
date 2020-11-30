# [剑指 Offer 11. 旋转数组的最小数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

## 题目描述



把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

示例 1：

输入：[3,4,5,1,2]
输出：1
示例 2：

输入：[2,2,2,0,1]
输出：0

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. 暴力求解

数组本身是递增排序的，经过一次旋转。变成两段递增排序的数组，整体趋势是先递增，然后在某处（最小元素）突然递减，然后再递增。现在只需要扫描一次找到突然递减的位置即可。

时间复杂度为：$O(n)$

空间复杂度为：$O(1)$

```c++
class Solution {
public:
    int minArray(vector<int>& numbers) {
        int min_ele = numbers[0];//最小元素

        for(int i = 1; i< numbers.size(); ++i){
            if(numbers[i] < min_ele){//出现元素比最小元素小，即突然递减的位置，直接返回即可。
                return numbers[i];
            }
        }
        return min_ele;
    }
};
```



2. 二分法

虽然不是整体有序，但也能用二分法求解问题，low与high分别表示二分的上界与下界，mid为中间的位置。若$numbers[mid] < numbers[high]$，则mid处元素位于最小值的右侧，可能为最小值，下一步搜索区间为$[low, mid]$，若$numbers[mid] > numbers[high]$，则mid处元素位于最小值的左侧，下一步搜索区间为$[mid+1, high]$，若$numbers[mid] == numbers[high]$，则无法直接判断$mid$处于最小值的哪一侧，只能微调$high$。

```c++
/*
二分法
时间复杂度为O(logn)
*/
class Solution {
public:
    int minArray(vector<int>& numbers) {
        int len = numbers.size();
        int low = 0, high = len - 1;
        int mid = low + (high - low) / 2;
        while(low <= high){
            mid = low + (high - low) / 2;
            if(numbers[mid] < numbers[high]){//位于最小值右侧
                high = mid;
            }
            else if(numbers[mid] > numbers[high]){//位于最小值左侧
                low = mid + 1;
            }
            else{//相同的话，无法确定其与最小值的位置关系，只能微调high
                if(numbers[mid] < numbers[low]){//位于最小值右侧
                    high = mid;
                }
                else{
                    --high;
                }   
            }
        }
        return numbers[low];
    }
};
```

