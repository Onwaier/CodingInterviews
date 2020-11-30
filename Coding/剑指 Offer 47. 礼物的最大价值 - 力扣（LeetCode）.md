# [剑指 Offer 47. 礼物的最大价值 - 力扣（LeetCode）](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)

## 题目描述

在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

 

示例 1:

输入: 
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 12
解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物


提示：

0 < grid.length <= 200
0 < grid[0].length <= 200

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

**动态规划**

设$dp[i][j]$表示棋盘$(i,j)$处可以获得的最大价值的礼物。则状态转换方程为：

$dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]) + gird[i][j]$，对于$i-1$与$j-1$为负值时，要单独处理。

为了节省空间，可以用$grid[i][j]$去存储$dp[i][j]$，这样可以节省空间，但也修改原数组的值，这是不好的。

为了保证计算$dp[i][j]$时，$dp[i-1][j]$与$dp[i][j-1]$是最新值，遍历时应从左向右，从上到小进行。

时间复杂度为：$O(mn)$。

空间复杂度为：O(1)，若不想修改原数组，可以用一个一维数组来存储dp值，空间复杂度为$O(n)$。

```cpp
/*
时间复杂度为：O(mn)
空间复杂度为：O(1)
*/

class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        int rows = grid.size();//行数
        int cols = grid[0].size();//列数

        for(int i = 0; i < rows; ++i){
            for(int j = 0; j < cols; ++j){
                int num1 = i - 1 < 0?0:grid[i - 1][j];
                int num2 = j - 1 < 0?0:grid[i][j - 1];
                grid[i][j] = max(num1, num2) + grid[i][j];
            }
        }

        return grid[rows - 1][cols - 1];
    }
};
```

