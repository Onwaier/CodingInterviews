# [剑指 Offer 24. 反转链表 - 力扣（LeetCode）](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

## 题目描述

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

 

示例:

输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL


限制：

0 <= 节点个数 <= 5000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. 双指针法

链表的常见构建方式有两种：头插法和尾插法。尾插法元素在链表的位置与插入顺序保持一致，而头插法中元素顺序则完全相反。可以利用这一个特点，用头插法重新构建链表达到反转链表的效果。

```cpp
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
    ListNode* reverseList(ListNode* head) {
        ListNode* p = head, *tmp;
        head = NULL;
        while(p != NULL){
            tmp = p->next;//备份
            p->next = head;//指向头指针
            head = p;//更新头指针的指向
            p = tmp;//遍历链表的下一个元素
        }
        return head;
    }
};
```

2. 递归法

```cpp
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
    ListNode* reverseList(ListNode* head) {
        if (head == NULL || head->next == NULL) {
            return head;
        }
        ListNode* ret = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return ret;
    }
};
```

