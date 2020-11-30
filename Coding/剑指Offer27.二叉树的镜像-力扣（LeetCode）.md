# [剑指 Offer 27. 二叉树的镜像  - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

## 题目描述

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：

​	  4

   /     \
  2      7
 / \     / \
1   3 6   9
镜像输出：

 	 4

   /     \
  7     2
 / \     / \
9 6    3 1

 

示例 1：

输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]


限制：

0 <= 节点个数 <= 1000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. 递归法

递归遍历二叉树，交换每个结点的左右子结点，这里从上向下和从下向上交换都是可以的。

代码采用的是从上往下交换

时间复杂度为$O(n)$

空间复杂度为$O(n)$，最坏的情况是二叉树退化成一个链表。

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
    TreeNode* mirrorTree(TreeNode* root) {
        if(root != NULL){
            swap(root->left, root->right);
            mirrorTree(root->left);
            mirrorTree(root->right);
        }
        return root;
    }
};
```



2. 栈或队列模拟

这里采用的是层次遍历来遍历二叉树，然后交换每个结点的左右子结点。

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
 层次遍历交换每个结点的左右子结点
 时间复杂度为：O(n)
 空间复杂度为：O(n)
 */
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        queue<TreeNode*>que;
        if(root != NULL){
            que.push(root);
        }
        TreeNode* now, *next;
        while(!que.empty()){
            now = que.front();
            que.pop();
            if(now->left != NULL){
                que.push(now->left);
            }
            if(now->right != NULL){
                que.push(now->right);
            }
            swap(now->left, now->right);
        }
        return root;
    }
};
```



