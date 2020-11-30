# [剑指 Offer 13. 机器人的运动范围 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)



## 题目描述

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

 

示例 1：

输入：m = 2, n = 3, k = 1
输出：3
示例 2：

输入：m = 3, n = 1, k = 0
输出：1
提示：

1 <= n,m <= 100
0 <= k <= 20

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. BFS

```c++
class Solution {
public:
    int movingCount(int m, int n, int k) {
        queue<pair<int, int>>que;//定义队列
        bool mark[105][105];//定义标记数组
        int dir[4][2] = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};//定义方向
        int cnt = 0;//定义并初始化计数器
        pair<int, int>now, next;
        memset(mark, false, sizeof(mark));//标记数组初始化

        que.push(make_pair(0, 0));//将起点加入队列
        cnt = cnt + 1;//计数加1
        mark[0][0] = true;//标记起点访问过
        while(!que.empty()){
            now = que.front();
            que.pop();

            for(int i = 0; i < 4; ++i){//四个方向移动
                next.first = now.first + dir[i][0];
                next.second = now.second + dir[i][1];

                if(next.first >= 0 && next.first < m && next.second >= 0 && next.second < n && !mark[next.first][next.second] && judge(next.first, next.second, k)){//不越界，未访问且数位和不大于k
                    mark[next.first][next.second] = true;//标记
                    que.push(next);//入队
                    cnt = cnt + 1;//计算器+1
                }
            }
        }
        return cnt;
        
    }
    bool judge(int m, int n, int k){//计算m与n的数位和
        int sum = 0;
        while(m){
            sum = sum + m % 10;
            m = m / 10;
        }
        while(n){
            sum = sum + n % 10;
            n = n / 10;
        }
        return sum <= k;
    }
};
```



2. DFS

```c++
/*
DFS
时间复杂度为：O(mn)
空间复杂度为：O(mn)
*/

class Solution {
public:
    bool mark[105][105];
    int movingCount(int m, int n, int k) {
        memset(mark, false, sizeof(mark));//标记数组初始化
        return dfs(0, 0, m, n, k);
    }
    bool judge(int m, int n, int k){//判断m与n的数位和是否小于k
        int sum = 0;

        while(m){
            sum = sum + m % 10;
            m = m / 10;
        }
        while(n){
            sum = sum + n % 10;
            n = n / 10;
        }

        return sum <= k;
    }

    int dfs(int i, int j, int m, int n, int k){//dfs
        if(i >= 0 && i < m && j >= 0 && j < n && !mark[i][j] && judge(i, j, k)){
            mark[i][j] = true;
            return 1 + dfs(i + 1, j, m, n, k) + dfs(i, j + 1, m, n, k) + dfs(i - 1, j, m, n, k) + dfs(i, j - 1, m, n, k);
        }
        else{
            return 0;
        }
    }
};
```

