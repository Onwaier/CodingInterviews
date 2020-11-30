# [剑指 Offer 12. 矩阵中的路径 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)



## 题目描述

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

 

示例 1：

输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
示例 2：

输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
提示：

1 <= board.length <= 200
1 <= board[i].length <= 200

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. DFS暴力求解

先扫描矩阵元素，当某个元素与字符数组的第一个字符相同时，开始从当前字符搜索，搜索所有长度为len的路径中是否有匹配当前字符数组的。有，返回true,不再继续搜索。若无，则继续扫描矩阵，找寻匹配字符数组第一个字符的元素，如此重复，直到匹配到字符数组，若都不能，则返回false.

```c++
class Solution {
public:
    bool mark[205][205];
    bool flag = false;
    bool exist(vector<vector<char>>& board, string word) {
        bool flag = false;
        int rows = board.size();//矩阵的行数
        int cols = board[0].size();//矩阵的列数
        int len = word.size();//word的长度
        

        for(int i = 0; i < rows; ++i){
            for(int j = 0; j < cols; ++j){
                if(board[i][j] == word[0]){//匹配word的起点
                    mark[i][j] = true;//标记
                    if(dfs(board, word, i, j, 1)){//dfs从指定长度的所有路径中匹配符合题意的
                        return true;
                    }
                    mark[i][j] = false;//环境恢复
                }
            }
        }
        return false;
    }

    bool dfs(vector<vector<char>>& board, string& word, int x, int y, int depth){
        int rows = board.size();//矩阵的行数
        int cols = board[0].size();//矩阵的列数
        int len = word.size(), idx = 1;//word的长度
        int dir[4][2] = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};
        int next_x, next_y;
        bool res = false;
        if(flag){//找到一条路径 就return  不再浪费时间
            return true;
        }
        if(depth >= len){
            flag = true;
            return true;
        }
        for(int i = 0; i < 4; ++i){
            next_x = x + dir[i][0];
            next_y = y + dir[i][1];
            if(next_x >= 0 && next_x < rows && next_y >= 0 && next_y < cols && !mark[next_x][next_y] && board[next_x][next_y] == word[depth]){
                mark[next_x][next_y] = true;
                res = dfs(board, word, next_x, next_y, depth + 1);
                mark[next_x][next_y] = false;//环境恢复
            }
        }
        return res;
    }
};
```

