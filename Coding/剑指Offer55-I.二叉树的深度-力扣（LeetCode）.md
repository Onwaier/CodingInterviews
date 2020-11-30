# [剑指 Offer 55 - I. 二叉树的深度 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

## 题目描述

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

例如：

给定二叉树 [3,9,20,null,null,15,7]，

​    3

   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。

 

提示：

节点总数 <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

**DFS**

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
 /*
 时间复杂度为：O(n)
 空间复杂度为：O(n)
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(NULL == root){
            return 0;
        }
        else{
            return 1 + max(maxDepth(root->left), maxDepth(root->right));
        }
    }
};
```

**BFS**

利用双向队列，按层遍历

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
    int maxDepth(TreeNode* root) {
        deque<TreeNode*>que;

        if(NULL == root){
            return 0;
        }
        que.push_back(root);
        TreeNode* frt = NULL;
        int level = 0;
        while(!que.empty()){
            ++level;
            for(int i = que.size(); i > 0; --i){
                frt = que.front();
                que.pop_front();
                if(NULL != frt->left){
                    que.push_back(frt->left);
                }
                if(NULL != frt->right){
                    que.push_back(frt->right);
                }
            }
        }
        return level;
    }
};
```

