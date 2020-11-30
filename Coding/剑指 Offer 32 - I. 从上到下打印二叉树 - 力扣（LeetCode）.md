# [剑指 Offer 32 - I. 从上到下打印二叉树 - 力扣（LeetCode）](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

## 题目描述

从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

 

例如:
给定二叉树: [3,9,20,null,null,15,7],

​	3

   / \
  9  20
      /  \
   15   7
返回：

[3,9,20,15,7]


提示：

节点总数 <= 1000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

层次遍历，队列实现。

时间复杂度为：$O(n)$，n为结点数

空间复杂度为：O(n)，最差情况下，即当树为平衡二叉树时，最多有 N/2 个树节点**同时**在 `queue` 中，使用 O(N)大小的额外空间。

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
    vector<int> levelOrder(TreeNode* root) {
        vector<int>res;
        queue<TreeNode*>que;
        TreeNode *now, *next;
        if(root != NULL)
            que.push(root);
        while(!que.empty()){
            now = que.front();
            que.pop();
            res.push_back(now->val);//编历一个结点
            if(now->left){
                que.push(now->left);//添加左结点
            }
            if(now->right){
                que.push(now->right);//添加右结点
            }
        }
        return res;
    }
};
```

