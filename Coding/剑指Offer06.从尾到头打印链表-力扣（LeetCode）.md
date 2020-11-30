# [剑指 Offer 06. 从尾到头打印链表 - 力扣（LeetCode）](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

## 题目描述

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

 

示例 1：

输入：head = [1,3,2]
输出：[2,3,1]


限制：

0 <= 链表长度 <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解



1. reverse函数

遍历一遍链表，将元素存储在vector中，然后再使用 **reverse**函数反转数组.

时间复杂度为：$O(n)$，空间复杂度为：$O(n)$.

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */

 /*
 遍历链表
 用数组存储每个元素，然后反转数组
 时间复杂度为：O(n)
 空间复杂度为：O(n)
 */
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        vector<int>res;
        ListNode* p = head;
        while(p != NULL){//遍历链表
            res.push_back(p->val);
            p = p -> next;//移动指针
        }
        reverse(res.begin(), res.end());//反转数组
        return res;
    }
};
```



2. [递归法](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/solution/mian-shi-ti-06-cong-wei-dao-tou-da-yin-lian-biao-d/)

用递归实现节点的逆序输出. 先一直向下递归，直到遇到空指针，然后在回溯的过程，将元素添加到vector中.

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */

 /*
 递归法
 时间复杂度：O(n)
 空间复杂度：O(n)
 */
class Solution {
public:
    vector<int>res;//res定义成成员变量
    vector<int> reversePrint(ListNode* head) {
        res.clear();//清空数组
        solve(head);
        return res;
    }
    void solve(ListNode* head){
        if(head == NULL){//遇到空指针返回
            return;
        }
        solve(head->next);//递归到下一层
        res.push_back(head->val);//回溯时添加元素  实现逆序
    }
    
};
```



3. 辅助栈

利用栈的特点。先遍历链表将元素全部压入栈，再弹出。便能实现元素的逆序.

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */

 /*
 辅助栈
 时间复杂度：O(n)
 空间复杂度：O(n)
 */
class Solution {
public:
    
    vector<int> reversePrint(ListNode* head) {
        stack<int>sta;//栈
        vector<int>res;//存储结果
        ListNode* p = head;
        while(p != NULL){//将链表中所有元素入栈
            sta.push(p->val);
            p = p->next;
        }
        while(!sta.empty()){//将所有元素出栈
            res.push_back(sta.top());
            sta.pop();
        }
        return res;
    }
    
    
};
```

