# [剑指 Offer 22. 链表中倒数第k个节点 - 力扣（LeetCode）](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

## 题目描述

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

 

示例：

给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

### 双指针法

定义两个指针 **p1**与 **p2**，初始时都指向第一个结点。其中指针**p1**先移动k步，即指向第k+1个结点。然后两个指针同时移动，当指针 **p1**指向链表的结尾时，表明指针**p2**此时指向的就是链表的倒数第k个结点。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode* p1, *p2;
        p1 = p2 = head;
        int cnt = 0;
        while(p1 != NULL && cnt < k){//p1先移动,指向到第k+1个结点
            p1 = p1->next;
            ++cnt;
        }
        while(p1 != NULL){//同时移动,当p1指向NULL时,p2指向倒数第k个结点
            p2 = p2->next;
            p1 = p1->next;
        }
        return p2;
    }
};
```



