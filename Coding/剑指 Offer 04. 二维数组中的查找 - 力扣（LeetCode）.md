# [剑指 Offer 04. 二维数组中的查找 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/submissions/)

[TOC]

## 题目描述

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。



示例:

现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
给定 target = 5，返回 true。

给定 target = 20，返回 false。

 

限制：

0 <= n <= 1000

0 <= m <= 1000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解



1. 暴力求解

遍历整个二维数组，找到就返回true，未找到应返回false。空间复杂度为：$O(1)$,时间复杂度为：$O(m*n)$。

```c++

class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        
        int rows = matrix.size();

        if(rows == 0){//空数组（特殊情况，特殊处理）
            return false;
        }
        int cols = matrix[0].size();

        //二维循环
        for(int i = 0; i < rows; ++i){
            for(int j = 0; j < cols; ++j){
                if(matrix[i][j] == target){//找到目标，返回true
                    return true;
                }
            }
        }
        return false;
    }
};
```



2. 行与列同时二分查找

因为行元素与列元素都是有序的，可以利用这一特点，每次对行与列交替二分查找，这样大大减少搜索时间。

```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        
        int rows = matrix.size();

        if(rows == 0){//空数组（特殊情况，特殊处理）
            return false;
        }
        int cols = matrix[0].size();

        int r_begin = 0, r_end = rows - 1;//记录搜索行的起始与终止位置
        int c_begin = 0, c_end = cols - 1;//记录搜索列的起始与终止位置
        int begin = c_begin, end = c_end, mid;
        while(c_begin <= c_end && r_begin <= r_end){//当行搜索完或列搜索完跳出循环
            //列二分搜索
            begin = c_begin, end = c_end;
            while(begin <= end){//二分搜索
                mid = (begin + end) / 2;
                if(matrix[r_begin][mid] == target){
                    return true;
                }
                else if(matrix[r_begin][mid] < target){
                    begin = mid + 1;
                }
                else{
                    end = mid - 1;
                }
            }
            c_end = end;//更新搜索列的终止位置
            r_begin += 1;//执行到这里，表示当前行未找到元素，下次应从下一行开始查找
            //行二分搜索
            begin = r_begin, end = r_end;
            while(begin <= end){//二分查找
                mid = (begin + end) / 2;
                if(matrix[mid][c_begin] == target){//找到目标  返回true
                    return true;
                }
                else if(matrix[mid][c_begin] < target){
                    begin = mid + 1;
                }
                else{
                    end = mid - 1;
                }
            }
            r_end = end;//更新搜索行的终止位置
            c_begin += 1;//执行到这里，表示当前列未找到元素，下次应从下一列开始查找
        }
        return false;
    }
};
```

![image-20200727193402239](https://raw.githubusercontent.com/onwaiers/Picture/master/img/20200727193414.png)

3. 二叉搜索树

从右上角看，数据的分布就像一棵二叉搜索树，采用二叉搜索的查找方式即可。时间复杂度为树的高度即$O(m + n)$。

```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        
        int rows = matrix.size();

        if(rows == 0){//空数组（特殊情况，特殊处理）
            return false;
        }
        int cols = matrix[0].size();

        int i = 0, j = cols - 1;
        while(i < rows && j >= 0){
            if(target < matrix[i][j]){//target比当前元素小，应位于左边区域，列右移
                --j;
            }
            else if(target > matrix[i][j]){//target比当前元素大，应位于下方区域，行下移
                ++i;
            }
            else{//找到元素
                return true;
            }
        }
        return false;//未找到元素
    }
};
```

