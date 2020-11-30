# [剑指 Offer 35. 复杂链表的复制 - 力扣（LeetCode）](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

## 题目描述

请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。

 

示例 1：

输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e1.png)

示例 2：

输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e2.png)

示例 3：

输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e3.png)

示例 4：

输入：head = []
输出：[]
解释：给定的链表为空（空指针），因此返回 null。




提示：

-10000 <= Node.val <= 10000
Node.random 为空（null）或指向链表中的节点。
节点数目不超过 1000 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. 两次遍历。

* 第一次遍历，构造新结点并建立next关系。遍历过程需：用**vector**存放新结点的位置和**unordered_map**建立原始链表的结点与序号的关系。
* 第二次遍历，先用**unordered_map**查询当前结点的random指针指向的结点对应的序号，再在**vector**中去查找新链表的对应序号的结点的位置，建立random关系。

时间复杂度为：$O(n)$，两次遍历。

空间复杂度为：$O(n)$，vector与unordered_map存放元素的个数等于链表结点个数。

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        //先扫描一遍 仅复制val和next
        Node *p1, *p2, *res;
        vector<Node*>vec;
        unordered_map<Node*, int>myMap;//建立节点的random与序号的索引
        int cnt = 0;
        p1 = head;
        p2 = res = NULL;
        while(p1 != NULL){//遍历链表，先忽略random 存放节点指针
            myMap[p1] = cnt++;
            Node* tmp = new Node(p1->val);
            vec.push_back(tmp);
            if(res == NULL){
                res = p2 = tmp;
            }
            else{
                p2->next = tmp;
                p2 = tmp;
            }
            p1 = p1->next;
        }
        myMap[NULL] = cnt;
        vec.push_back(NULL);
        if(p2 != NULL)
            p2->next = NULL;

        p1 = head;
        p2 = res;
        while(p1 != NULL){//再次遍历链表
            int idx = myMap[p1->random];
            p2->random = vec[idx];
            p1 = p1->next;
            p2 = p2->next;
        }
        return res;
    }
};
```

2. DFS和BFS

参考[题解](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/solution/lian-biao-de-shen-kao-bei-by-z1m/)

将复杂链表看成一个有向图，遍历图过程中，复制节点。注意已经创建的结点不要重复创建，用map去存放已经创建的结点。

* DFS

```cpp
//DFS
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
/*
将这个复杂的链表看成一个图
边遍历图边复制
遍历方式常见的有BFS和DFS
*/
class Solution {
public:
    unordered_map<Node*, Node*>vis;//存放已新建的结点，避免重复
    Node* copyRandomList(Node* head) {
        if(head == NULL){
            return NULL;
        }
        if(vis.find(head) != vis.end()){//结点已构建，直接返回
            return vis[head];
        }
        Node* p = new Node(head->val);
        vis[head] = p;
        p->next = copyRandomList(head->next);//DFS
        p->random = copyRandomList(head->random);//DFS
        return p;
    }
};
```

* BFS

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
/*
将这个复杂的链表看成一个图
边遍历图边复制
遍历方式常见的有BFS和DFS
*/
class Solution {
public:
    unordered_map<Node*, Node*>vis;//存放已新建的结点，避免重复
    Node* copyRandomList(Node* head) {
        queue<Node*>que;
        Node *frt = NULL, *res = NULL;
        if(head == NULL){
            return res;
        }
        res = new Node(head->val);
        vis[head] = res;
        que.push(head);
        while(!que.empty()){
            frt = que.front();
            que.pop();
            if(frt->next != NULL && vis.find(frt->next) == vis.end()){//next节点
                vis[frt->next] = new Node(frt->next->val);
                que.push(frt->next);
            }
            if(frt->random != NULL && vis.find(frt->random) == vis.end()){//random节点
                vis[frt->random] = new Node(frt->random->val);
                que.push(frt->random);
            }

            vis[frt]->next = vis[frt->next];//建立next关系
            vis[frt]->random = vis[frt->random];//建立random关系
        }
        return res;
    }
};
```

3. 迭代

参考[题解](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/solution/lian-biao-de-shen-kao-bei-by-z1m/)，遍历链表，复制结点，结点的next结点和结点的random结点，当结点已建立就不再复制，直接建立链接即可。

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
/*
迭代
*/
class Solution {
public:
    unordered_map<Node*, Node*>vis;//存放已新建的结点，避免重复
    Node* copyRandomList(Node* head) {
        queue<Node*>que;
        Node *tmp = head, *res = NULL;
        if(tmp == NULL){
            return NULL;
        }
        res = new Node(tmp->val);
        vis[tmp] = res;
        while(tmp != NULL){//遍历
            if(tmp->next != NULL && vis.find(tmp->next) == vis.end()){
                vis[tmp->next] = new Node(tmp->next->val);
            }
            if(tmp->random != NULL && vis.find(tmp->random) == vis.end()){
                vis[tmp->random] = new Node(tmp->random->val);
            }
            vis[tmp]->next = vis[tmp->next];
            vis[tmp]->random = vis[tmp->random];
            tmp = tmp->next;//next结点
        }
        return res;
    }
};
```



