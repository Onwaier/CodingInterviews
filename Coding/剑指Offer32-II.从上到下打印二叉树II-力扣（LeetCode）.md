# [剑指 Offer 32 - II. 从上到下打印二叉树 II - 力扣（LeetCode）](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

## 题目描述

从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

 

例如:
给定二叉树: [3,9,20,null,null,15,7],

​	3

   / \
  9  20
      /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]


提示：

节点总数 <= 1000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

同[剑指offe32-1](剑指 Offer 32 - I. 从上到下打印二叉树 - 力扣（LeetCode）.md)，都是层次遍历二叉树，不同的是，需要将同一层的元素放在一个数组中，为了标记节点层数，除了将指向节点的指针加入队列，还需要将层次信息加入进去，这里可以使用结构体或者stl中的 **pair**来实现，具体内容见代码：

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>>res;
        if(root == NULL){
            return res;
        }
        queue<pair<TreeNode*, int>>que;
        int level = 0;
        vector<int>tmp;
        pair<TreeNode*, int>now;
        que.push(make_pair(root, 0));
        while(!que.empty()){
            now = que.front();
            que.pop();
            if(now.second != level){//当前结点为下一层
                res.push_back(tmp);
                tmp.clear();//清空
                level = now.second;//更新当前层数（即+1）
            }
            tmp.push_back(now.first->val);
            
            if(now.first->left != NULL){//左结点非空
                que.push(make_pair(now.first->left, now.second + 1));
            }
            if(now.first->right != NULL){//右结点非空
                que.push(make_pair(now.first->right, now.second+1));
            }
        }
        res.push_back(tmp);
        return res;
    }
};
```



