# [剑指 Offer 36. 二叉搜索树与双向链表 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

## 题目描述

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

为了让您更好地理解问题，以下面的二叉搜索树为例：

 ![img](https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png)



 

我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。

下图展示了上面的二叉搜索树转化成的链表。“head” 表示指向链表中有最小元素的节点。

 ![img](https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png)

特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

易知对二叉搜索树中序遍历，能保证遍历的元素有序，另建立双向链表时，当前结点now，与前一结点pre的关系为`now->left=pre, pre->right=now`。所以我们需存储pre结点，当pre为NULL,表示此时结点now为头结点head，更新`head=now`。中序遍历完成后，还需建立头结点与尾结点的关系，即`now->right=pre, pre->left=now`。

时间复杂度与空间复杂度均为$O(n)$。

具体代码如下：

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
public:
    Node* pre, *head;
    Node* treeToDoublyList(Node* root) {
       head = pre = NULL;
       if(root == NULL){
           return NULL;
       }
       dfs(root);
       head->left = pre;//头结点指向尾结点
       pre->right = head;//尾结点指向头结点
       return head;
    }

    void dfs(Node* now){
        if(now == NULL){
            return;
        }
        dfs(now->left);
        if(pre == NULL){//双向链表的头结点
            head = pre = now;
        }
        else{
            now->left = pre;//后指前
            pre->right = now;//前指后
            pre = now;//更新pre结点
        }
        dfs(now->right);

    }
};
```

