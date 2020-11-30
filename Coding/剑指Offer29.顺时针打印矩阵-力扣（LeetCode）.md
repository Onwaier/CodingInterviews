# [剑指 Offer 29. 顺时针打印矩阵 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

## 题目描述

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

 

示例 1：

输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
示例 2：

输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]


限制：

0 <= matrix.length <= 100
0 <= matrix[i].length <= 100

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

模拟法，模拟顺时针填数的过程。填数的方向是右->下->左->上->...，如此循环往复，注意的一点时，每次调整方向的同时也要调整边界。

时间复杂度为$O(m*n)$，m为矩阵的行数，n为矩阵的列数。

空间复杂度为$O(1)$。

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int>res;
        int rows = matrix.size();
        if(rows == 0){//空数组直接返回
            return res;
        }
        int cols = matrix[0].size();
        int dir[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        int x = 0, y = -1, nextX, nextY, idx = 0;
        int xStart = 0, xEnd = rows - 1, yStart = 0, yEnd = cols - 1;
        for(int t = 0; t < rows * cols; ++t){
            for(int i = 0; i < 4; ++i){//调整方向
                idx = (idx + i) % 4;
                if(i != 0){
                    idx == 0?yStart++:idx == 1?xStart++:idx == 2?yEnd--:xEnd--;//调整边界（相当于标记数组的作用）
                }
                nextX = x + dir[idx][0];
                nextY = y + dir[idx][1];
                if(nextX >= xStart && nextX <= xEnd && nextY >= yStart && nextY <= yEnd){
                    x = nextX;//更新位置
                    y = nextY;
                    res.push_back(matrix[x][y]);
                    break;
                }
            }
        }
        return res;
    }
};
```

优化。同一方向用for循环，一直填数到边界，四个方向：从左到右，从上到下，从右到左，从下到上，同时更新边界，直到无处可填为止。

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int>res;
        int rows = matrix.size();
        if(rows == 0){//空数组直接返回
            return res;
        }
        int cols = matrix[0].size();
        int xStart = 0, xEnd = rows - 1, yStart = 0, yEnd = cols - 1 ,x, y;
        while(true){
            //从左向右
            for(int x = xStart, y = yStart; y <= yEnd; ++y){
                res.push_back(matrix[x][y]);
            }
            if(++xStart > xEnd){
                break;
            }

            //从上向下
            for(int x = xStart, y = yEnd; x <= xEnd; ++x){
                res.push_back(matrix[x][y]);
            }
            if(--yEnd < yStart){
                break;
            }

            //从右向左
            for(int x = xEnd, y = yEnd; y >= yStart; --y){
                res.push_back(matrix[x][y]);
            }
            if(--xEnd < xStart){
                break;
            }

            //从下向上
            for(int x = xEnd, y = yStart; x >= xStart; --x){
                res.push_back(matrix[x][y]);
            }
            if(++yStart > yEnd){
                break;
            }
        }
        return res;
    }
};
```

