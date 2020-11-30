# [剑指 Offer 37. 序列化二叉树 - 力扣（LeetCode）](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)

## 题目描述

请实现两个函数，分别用来序列化和反序列化二叉树。

示例: 

你可以将以下二叉树：

​	1

   / \
  2   3
      / \
    4   5

序列化为 "[1,2,3,null,null,4,5]"

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

序列化二叉树相当于层次遍历，注意序列的格式。

反序列化二叉树是将序列生成一棵二叉树，只需要利用队列将序列扫描一遍即可完成二叉树的创建。

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
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {//层次遍历（注意序列的格式）
        string res = "[";
        queue<TreeNode*>que;
        que.push(root);
        TreeNode* frt;
        string tmp;

        while(!que.empty()){
            frt = que.front();
            que.pop();
            frt == root?tmp = "":tmp = ",";
            frt == NULL?tmp += "null":tmp += to_string(frt->val);
            res += tmp;
            if(frt != NULL){
                que.push(frt->left);
                que.push(frt->right);
            }
        }

        res.push_back(']');
        return res;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {//用序列生成一棵二叉树
        TreeNode *root = NULL, *frt = NULL;
        string tmp = data.substr(1, data.size() - 2);
        vector<string>res;
        if(tmp == ""){
            return NULL;
        }
        int idx = 0, idx2 = 0;
        while((idx = tmp.find(',', idx2)) != EOF){
            res.push_back(tmp.substr(idx2, idx - idx2));
            idx2 = idx + 1;
        }
        res.push_back(tmp.substr(idx2));

        queue<TreeNode*>que;
        idx = 0;
        if(res[idx] != "null"){
            root = new TreeNode(stoi(res[idx++]));
            que.push(root);
        }

        while(!que.empty()){
            frt = que.front();
            que.pop();
            if(idx < res.size() && res[idx] != "null"){
                frt->left = new TreeNode(stoi(res[idx]));
                que.push(frt->left);
            }
            ++idx;
            if(idx < res.size() && res[idx] != "null"){
                frt->right = new TreeNode(stoi(res[idx]));
                que.push(frt->right);
            }
            ++idx;
        }

        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));
```

