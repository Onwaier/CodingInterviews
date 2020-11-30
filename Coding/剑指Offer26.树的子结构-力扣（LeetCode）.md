# [剑指 Offer 26. 树的子结构 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

## 题目描述

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:
给定的树 A:

​      3

​    /  \

   4   5
  / \
 1   2
给定的树 B：

   4 
  /
 1
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。

示例 1：

输入：A = [1,2,3], B = [3,1]
输出：false
示例 2：

输入：A = [3,4,5,1,2], B = [4,1]
输出：true
限制：

0 <= 节点个数 <= 10000



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

先遍历树A，然后判断B是否是当前结点下的子结构。



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
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if(A != NULL && B == NULL){
            return false;
        }
        if(!isSame(A, B)){
            if(A == NULL || B == NULL){
                return false;
            }
            else{
                return isSubStructure(A->left, B) || isSubStructure(A->right, B);
            }
        }
        else{
            return true;
        }
    }
    bool isSame(TreeNode* A, TreeNode* B){
        if((A == NULL) && (B != NULL)){
            return false;
        }
        else if(B == NULL){
            return true;
        }
        else{
            return A->val == B->val && isSame(A->left, B->left) && isSame(A->right, B->right);
        }
        
    }
};
```

参考[题解](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/solution/mian-shi-ti-26-shu-de-zi-jie-gou-xian-xu-bian-li-p/)

时间复杂度为：$O(mn)$，m和n分别是树A与树B的结点数目，遍历树A的复杂度为$O(m)$，判断B与当前结点下的子结构的时间复杂度为$O(n)$，总的复杂度为$O(mn)$。

空间复杂度为：$O(n)$，最坏的情况是A即化成链表，且需要遍历到A的叶子结点。

正确代码如下：

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
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        return A != NULL && B != NULL && (isSame(A, B) || 
        isSubStructure(A->left, B) || isSubStructure(A->right, B));
    }
    bool isSame(TreeNode* A, TreeNode* B){
        if(B == NULL){
            return true;
        }
        else if(A == NULL || A->val != B->val){
            return false;
        }
        else{
            return isSame(A->left, B->left) && isSame(A->right, B->right);
        }
        
    }
};
```

我的代码A与B皆为空树时的返回结果是错误的，并且我的代码逻辑没有他的清晰。