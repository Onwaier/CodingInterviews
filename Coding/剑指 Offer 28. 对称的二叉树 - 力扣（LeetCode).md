# [剑指 Offer 28. 对称的二叉树 - 力扣（LeetCode)](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

## 题目描述

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

​      1

   /     \
  2     2
 / \    / \
3  4 4  3
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

​	1

   / \
  2   2
   \   \
   3    3

 

示例 1：

输入：root = [1,2,2,3,4,4,3]
输出：true
示例 2：

输入：root = [1,2,2,null,3,null,3]
输出：false


限制：

0 <= 节点个数 <= 1000



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

参考[题解](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/solution/mian-shi-ti-28-dui-cheng-de-er-cha-shu-di-gui-qing/)

> 对称二叉树定义： 对于树中 任意两个对称节点 L 和 R ，一定有：
> L.val = R.valL.val=R.val ：即此两对称节点值相等。
> L.left.val = R.right.val：即 L的 左子节点 和 R的 右子节点 对称；
> L.right.val = R.left.val：即 L的 右子节点 和 R的 左子节点 对称。

依此规律，从上往下递归即可。

时间复杂度为：O(n)

空间复杂度为：O(n)

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
 //参考题解 https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/solution/mian-shi-ti-28-dui-cheng-de-er-cha-shu-di-gui-qing/
 
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root == NULL) return true;
        else return judge(root->left, root->right);
    }
    bool judge(TreeNode* l, TreeNode* r){
        if(l == NULL && r == NULL){
            return true;
        }
        if(l == NULL || r == NULL || l->val != r->val){
            return false;
        }
        else{
            return judge(l->left, r->right) && judge(l->right, r->left);
        }
    }
};
```

