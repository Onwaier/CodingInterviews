# [剑指 Offer 34. 二叉树中和为某一值的路径 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

## 题目描述

输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

 

示例:
给定如下二叉树，以及目标和 sum = 22，
                  5
                /   \
            4        8
           /          / \
          11     13  4
          / \      / \
        7    2  5   1
返回:

[
   [5,4,11,2],
   [5,8,4,5]
]


提示：

节点总数 <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

采用**先序遍历**，遍历二叉树的所有路径，将其中路径中节点值和为给定整数的路径添加结果集中。

时间复杂度和空间复杂度同先序遍历为：$O(n)$

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
    vector<vector<int>>res;//存放结果
    vector<int>tmp;//存放当前节点路径
    int tmpSum;//路径和
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        if(root == NULL){
            return res;
        }
        tmpSum = 0;
        dfs(root, sum);
        return res;
    }
    void dfs(TreeNode* root, int sum){//采用先序遍历递归
        tmpSum += root->val;
        tmp.push_back(root->val);

        if(root->left != NULL){//遍历左子树
            dfs(root->left, sum);
        }
        if(root->right != NULL){//遍历右子树
            dfs(root->right, sum);
        }
        if(root->left == NULL && root->right == NULL && tmpSum == sum){
            res.push_back(tmp);
        }
        
        tmpSum -=root->val;//返回到上一层前恢复tmpSum与tmp的值（恢复环境）
        tmp.pop_back();
    }
};
```

