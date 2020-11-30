# [剑指 Offer 54. 二叉搜索树的第k大节点 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

## 题目描述

给定一棵二叉搜索树，请找出其中第k大的节点。

 

示例 1:

输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 4

示例 2:

输入: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 4


限制：

1 ≤ k ≤ 二叉搜索树元素个数

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

**递归法**

二叉搜索树的特点：左子树所有结点都小于根结点，而所有右子树结点都不小于根结点。

若`nodeNum(root)`表示树的结点数

当右子树有`k - 1`个结点时，因为这`k - 1`个结点的值都不于根结点的值，所以根结点的值就是第k大值。

当右子树的结点数小于`k - 1`，那第`k`大值应该在左子树，在左子树中，它应该算第`k - nodeNum(root->right) - 1 `大的数。

当右子树的结点数大于`k - 1`，那第k大值在右子树中，但不是当前根结点的值，需要继续递归右子树。

空间复杂度为：$O(n)$，退化成链表时，统计节点数为$O(n)$

时间复杂度为：$O(n^2)$，退化为链表时，返回第1大结点的值，递归深度为n，统计结点数为$O(n)$，所以总的时间复杂度为：$O(n)$。

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
    int kthLargest(TreeNode* root, int k) {
        int num = nodeNum(root->right);//右子树节点数
        if(num == k - 1){//为k-1就直接返回根结点
            return root->val;
        }
        else if(num < k - 1){//表明第k大的结点在左子树
            return kthLargest(root->left, k - num - 1);
        }
        else{//继续搜索右子树
            return kthLargest(root->right, k);
        }
    }

    int nodeNum(TreeNode* root){//以root为根结点的树的节点数
        if(NULL == root){
            return 0;
        }
        else{
            return 1 + nodeNum(root->left) + nodeNum(root->right);
        }
    }
};
```

**DFS（逆中序遍历 右中左）**

参考[题解](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/solution/mian-shi-ti-54-er-cha-sou-suo-shu-de-di-k-da-jie-d/)，中序遍历一棵二叉搜索树，得到的是递增序列，但按照**右->中->左**的顺序遍历，得到的是递减序列。

在遍历过程中来标记当前节点的序号，当序号等于**k**时，表示为第**k**大的结点，直接返回即可。

空间复杂度为：$O(n)$

时间复杂度为：$O(n)$，省于统计节点数的过程，效率比原算法高。

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
    int cnt = 0, res = 0;
    int kthLargest(TreeNode* root, int k) {
        dfs(root, k);
        return res;
    }
    void dfs(TreeNode* root, int k){
        if(cnt >= k || root == NULL){
            return;
        }
        dfs(root->right, k);
        if(++cnt == k){
            res = root->val;
            return;
        }
        dfs(root->left, k);
    }
};
```

