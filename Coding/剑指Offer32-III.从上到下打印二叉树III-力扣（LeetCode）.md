# [剑指 Offer 32 - III. 从上到下打印二叉树 III - 力扣（LeetCode）](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

## 题目描述

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

 

例如:
给定二叉树: [3,9,20,null,null,15,7],

​    3

   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [20,9],
  [15,7]
]


提示：

节点总数 <= 1000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

参考[剑指 Offer 32 - II](剑指 Offer 32 - II. 从上到下打印二叉树 II - 力扣（LeetCode）.md)，使用双向队列在层次遍历，对于偶数层先从左到右遍历，然后再反转，从而实现偶数层从右向左遍历。

时间复杂度为$O(n)$，空间复杂度为：$O(n)$，最差为平衡二叉树，最多存放n/2个元素。

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
        deque<TreeNode*>que;
        TreeNode* now;
        vector<vector<int>>res;
        if(root == NULL){
            return res;
        }
        vector<int>tmp;
        int level = 0;
        que.push_back(root);
        while(!que.empty()){ 
            tmp.clear();
            ++level;
            for(int i = que.size(); i > 0; --i){//注意这里for的写法
                now = que.front();
                que.pop_front();
                tmp.push_back(now->val);
                if(now->left != NULL){
                    que.push_back(now->left);
                }
                if(now->right != NULL){
                    que.push_back(now->right);
                }
            }
            if(!(level&1)){//偶数层反转
                reverse(tmp.begin(), tmp.end());
            }
            res.push_back(tmp);
        }
        return res;
    }
};
```

改进：不用反转。

1. 对于奇数层，从左向右遍历deque（front及pop_front()），依次将结点的左右子结点添加至队尾;
2. 对于偶数层，从右向左遍历deque（back及pop_back()）,依次将结点的右左子结点添加至队首。

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
        deque<TreeNode*>que;
        TreeNode* now;
        vector<vector<int>>res;
        if(root == NULL){
            return res;
        }
        vector<int>tmp;
        int level = 0;
        que.push_back(root);
        while(!que.empty()){ 
            tmp.clear();
            ++level;
            if(level&1){//奇数层 从左向右 front pop_front 左右
                for(int i = que.size(); i > 0; --i){
                    now = que.front();
                    que.pop_front();
                    tmp.push_back(now->val);
                    if(now->left != NULL){
                        que.push_back(now->left);
                    }
                    if(now->right != NULL){
                        que.push_back(now->right);
                    }
                }
            }
            else{//偶数层 从右向左 back push_back pop_back 右左
                for(int i = que.size(); i > 0; --i){
                    now = que.back();
                    que.pop_back();
                    tmp.push_back(now->val);
                    if(now->right != NULL){
                        que.push_front(now->right);
                    }
                    if(now->left != NULL){
                        que.push_front(now->left);
                    }
                }
            }
            res.push_back(tmp);
        }
        return res;
    }
};
```

