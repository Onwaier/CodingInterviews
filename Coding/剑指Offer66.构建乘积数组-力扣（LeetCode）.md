# [剑指 Offer 66. 构建乘积数组 - 力扣（LeetCode）](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/)

## 题目描述

给定一个数组 A[0,1,…,n-1]，请构建一个数组 B[0,1,…,n-1]，其中 B 中的元素 B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]。不能使用除法。

 

示例:

输入: [1,2,3,4,5]
输出: [120,60,40,30,24]


提示：

所有元素乘积之和不会溢出 32 位整数
a.length <= 100000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

**前缀与后缀**

如果没有 **不能使用除法**这一限制，我们可以求出数组所有元素的乘积，然后再分别除以每个元素即可。

$B[i]=A[0]×A[1]×\dots×A[i-1]×A[i+1]×\dots×A[n-1]$

$B[i]$的值可以两部分的乘积：$A[i]$前所有元素的乘积和$A[i]$后所有元素的乘积。

很容易想到的一个思路是分别用$prefix$数组 和 $suffix$数组存储前一部分乘积和后一部分乘积，而$prefix$和$suffix$数组都可以在$O(n)$的时间内得到。

$B[i] = prefix[i - 1] * suffix[i + 1]$。

时间复杂度与空间复杂度均为$O(n)$

```cpp
/*
用前缀数组和后缀数组存储a[i]结尾的连续元素乘积  和 以a[i]开始的连续元素乘积
时间复杂度为：O(n)
空间复杂度为：O(n)
*/
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        int len = a.size();

        vector<int>prefix(len, 1), suffix(len, 1);
        vector<int>res(len, 1);
        for(int i = 0, j = len - 1; i < len; ++i, --j){
            i == 0?prefix[i] = a[i]:prefix[i] = prefix[i - 1] * a[i];//计算prefix数组  prefix[i]
            j == len - 1?suffix[j] = a[j]:suffix[j] = suffix[j + 1] * a[j]; //计算suffix数组  suffix[j]
        }
        for(int i = 0; i < len; ++i){
            int num1 = (i == 0?1:prefix[i - 1]);
            int num2 = (i == len - 1?1:suffix[i + 1]);
            res[i] = num1 * num2;
        }
        return res;
    }
};
```

**空间优化**

能否将空间优化成$O(1)$，在计算$prefix$和 $suffix$同时，将其乘到对应的位置。

```cpp
/*
用前缀数组和后缀数组存储a[i]结尾的连续元素乘积  和 以a[i]开始的连续元素乘积
时间复杂度为：O(n)
空间复杂度为：O(1)
*/
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        int len = a.size();

        int prefix, suffix;
        vector<int>res(len, 1);
        for(int i = 0, j = len - 1; i < len; ++i, --j){

            //求前缀
            i == 0?prefix = a[i]:prefix = prefix * a[i];
            if(i + 1 < len) res[i + 1] *=  prefix; 

            //求后缀
            j == len - 1?suffix = a[j]:suffix = suffix * a[j];
            if(j - 1 >= 0) res[j - 1] *= suffix; 
        }
        return res;
    }
};
```



