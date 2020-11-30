# [剑指 Offer 33. 二叉搜索树的后序遍历序列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

## 题目描述

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。

 

参考以下这颗二叉搜索树：

​     5

​    / \

   2   6
  / \
 1   3
示例 1：

输入: [1,6,3,2,5]
输出: false
示例 2：

输入: [1,3,2,6,5]
输出: true


提示：

数组长度 <= 1000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. 递归法

如果只知道二叉树的后序遍历结果是无法确定一棵二叉树的形状的，这道题需要好好利用 **二叉搜索树**这一条件。

二叉搜索树有着“左子树的所有元素的值均小于根结点，右子树的所有元素的值均大于根结点”的特点，

利用这一特点去递归判断当前结点的左右子树的值是否合理（左小右大）即可。

时间复杂度为：$O(n^2)$，对于每一个结点都要调用一次judge，调用次数为n次，时间复杂度为：$O(n)$。而judge的最差情况是二叉树退化为一个链表，每次遍历节点数为$n-1, n-2, n-3, \dots, 0$，即$\frac{n * (n - 1)}{2}$，所以总的时间复杂度为$O(n^2)$.

空间复杂度为：$O(n)$ ,退化为链表，递归深度为n。

```cpp
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        return judge(postorder, 0, postorder.size());
    }
    bool judge(vector<int>& post, int start, int len){
        if(len <= 0){
            return true;
        }
        int rootVal = post[start + len - 1];
        int i = start;
        while(i < start + len - 1 && post[i] < rootVal) ++i;//左子树元素小于根节点
        int split = i - 1;//记录左子树与右子树的分界点（实际上是左子树的最后一个结点）
        while(i < start + len - 1 && post[i] > rootVal) ++i;//右子树元素大于根节点
        if(i == start + len - 1){//满足二叉搜索情况
            return judge(post, start, split - start + 1) && judge(post, split + 1, len - 1 - (split - start + 1));//递归 继续判断左子树与右子树是否符合二叉搜索情况
        }
        else{//不满足左小右大 直接返回false
            return false;
        }
    }
};
```

参照[题解](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/solution/mian-shi-ti-33-er-cha-sou-suo-shu-de-hou-xu-bian-6/)

2. 单调栈

```cpp
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
       stack<int>sta;
       int len = postorder.size();
       int root = INT_MAX;
       for(int i = len - 1; i >= 0; --i){
           if(postorder[i] > root){
               return false;
           }
           while(!sta.empty() && postorder[i] < sta.top()){
               root = sta.top();
               sta.pop();
           }
           sta.push(postorder[i]);
       } 
       return true;
    }
   
};
```

