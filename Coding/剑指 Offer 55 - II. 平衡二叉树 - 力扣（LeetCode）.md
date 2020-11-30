# [剑指 Offer 55 - II. 平衡二叉树 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

## 题目描述

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

 

示例 1:

给定二叉树 [3,9,20,null,null,15,7]

   3
   / \
  9  20
    /  \
   15   7
返回 true 。

示例 2:

给定二叉树 [1,2,2,3,3,null,null,4,4]

​      1
​      / \
​     2   2
​    / \
   3   3
  / \
4   4
返回 false 。

 

限制：

1 <= 树的结点个数 <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

1. 先序遍历 + 求树深度

这个是从上到下 判断每个结点

求树深度时，可以用map记录节点的高度，避免重复计算。但会增加内存开销。

时间复杂度为：$O(nlogn)$，最多需遍历n个结点，每个节点求高度为$logn$

空间复杂度为：$O(n)$

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
    unordered_map<TreeNode*, int>mark;
    bool isBalanced(TreeNode* root) {
        if(NULL == root){
            return true;
        }
        else{
            if(abs(getDepth(root->left) - getDepth(root->right)) <= 1){
                return isBalanced(root->left) && isBalanced(root->right);
            }
            else{
                return false;
            }
        }
    }

    int getDepth(TreeNode* root){
        if(NULL == root){
            return 0;
        }
        else{
            if(mark.find(root->left) == mark.end()){   
                mark[root->left] = getDepth(root->left);
            }
            if(mark.find(root->right) == mark.end()){
                mark[root->right] = getDepth(root->right);
            }
            return max(mark[root->left], mark[root->right]) + 1;
        } 
    }
};
```

2. 后序遍历

从下至上，判断每个结点对应的树是否是平衡二叉树。

时间复杂度为：$O(n)$，避免了重复计算。

空间复杂度为：$O(n)$。

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

 */
class Solution {
public:
    
    bool isBalanced(TreeNode* root) {
        if(NULL == root){
            return true;
        }
        return getDepth(root) != -1;
    }

    int getDepth(TreeNode* root){
        if(NULL == root){
            return 0;
        }
        int leftDepth = getDepth(root->left);
        if(leftDepth == -1){
            return -1;
        }
        int rightDepth = getDepth(root->right);
        if(rightDepth == -1){
            return -1;
        }
        return abs(leftDepth - rightDepth) < 2?max(leftDepth, rightDepth) + 1:-1;
    }
};
```

