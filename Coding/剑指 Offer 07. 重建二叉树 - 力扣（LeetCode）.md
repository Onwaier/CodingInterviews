# [剑指 Offer 07. 重建二叉树 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)



## 题目描述

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

 

例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：

    3
   / \
  9  20
    /  \
   15   7


限制：

0 <= 节点个数 <= 5000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解



1. 采用递归 + 分治思路。

根据先序遍历和中序遍历的特点，先序遍历的第一个元素即为根节点，而各元素不相同。扫描中序遍历，寻找根节点，根节点左侧的元素为左子树，右侧的元素为右子树。利用先序与中序序列递归构建左子树和右子树，便可构建整棵二叉树。



```c++
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
 递归 二分建立二叉树
 */
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        TreeNode* head = create(preorder.begin(), inorder.begin(), n);
        return head;
    }
    TreeNode* create(vector<int>::iterator pre, vector<int>::iterator inorder, int n){
        if(n <= 0){//数组中元素个数为0，返回NULL
            return NULL;
        }
        TreeNode* p = (TreeNode*)malloc(sizeof(TreeNode));
        p->val = *pre;
        for(vector<int>::iterator iter = inorder; iter < inorder + n; ++iter){
            if(*iter == p->val){//在中序序列中找到根节点，然后拆分成左子树和右子树
                int num = iter - inorder;//左子树的元素个数
                p->left = create(pre + 1, inorder, num);//左子树
                p->right = create(pre + num + 1, inorder + num + 1, n - num - 1);//右子树
                return p;
            }
        }
        return NULL;
    }
};
```



2. 递归 + map 索引inorder

与1不同的是，在inorder中寻找根元素，不再采用for循环遍历的方式，而是使用map建立值与下标的索引关系，实现$O$

```c++
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
 递归 分治建立二叉树
 */
class Solution {
public:
    unordered_map<int, int>dict;
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        for(int i = 0; i < n; ++i){
            dict[inorder[i]] = i;//建立中序序列中元素的字典，便于O(1)时间内完成查找
        }
        TreeNode* head = create(preorder.begin(), inorder.begin(), inorder.begin(), n);
        return head;
    }
    TreeNode* create(vector<int>::iterator pre, vector<int>::iterator in_start, vector<int>::iterator inorder, int n){
        if(n <= 0){//数组中元素个数为0，返回NULL
            return NULL;
        }
        TreeNode* p = (TreeNode*)malloc(sizeof(TreeNode));
        p->val = *pre;
        int idx = dict[p->val];//返回根节点在中序序列中位置
        int num = in_start + idx - inorder;//左子树元素个数
        p->left = create(pre + 1, in_start, inorder, num);
        p->right = create(pre + 1 + num, in_start, inorder + 1 + num , n - 1 - num);
        return p;
    }
};
```

