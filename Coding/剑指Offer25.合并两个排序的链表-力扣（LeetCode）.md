# [剑指 Offer 25. 合并两个排序的链表 - 力扣（LeetCode）](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

## 题目描述

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

示例1：

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
限制：

0 <= 链表长度 <= 1000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

1. 归并法

类似归并排序中的归并思路。

双指针，

* 循环合并。

  定义两个指针分别指向两个有序链表的头部，每次将较小val的结点添加到归并后的链表中，并移动对应指针，重复相同操作，直到某个指针移动到链表尾部为止。

* 合并剩余部分

  将指针指向的剩余链表。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */

 /*
 类似归并排序
 时间复杂度为:O(n)
 空间复杂度为:O(1)
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* res, *p1, *p2, *p;
        p1 = l1;
        p2 = l2;
        res = NULL;
        p = NULL;
        while(p1 != NULL && p2 != NULL){//循环合并
            if(p1->val < p2->val){
                if(res == NULL){//单独处理首结点的插入，较麻烦，后面同理
                    res = p1;
                    p = p1;
                }
                else{
                    p->next = p1;
                    p = p->next;
                }
                
                p1 = p1->next;
            }
            else{
                if(res == NULL){
                    res = p2;
                    p = p2;
                }
                else{
                    p->next = p2;
                    p = p->next;
                }
                p2 = p2->next;
            }
        }
        while(p1 != NULL){
            if(p == NULL){
                res = p = p1;
            }
            else{
                p->next = p1;
                p = p->next;
            }
            
            p1 = p1->next;
        }
        while(p2 != NULL){
            if(p == NULL){
                res = p = p2;
            }
            else{
                p->next = p2;
                p = p->next;
            }
            
            p2 = p2->next;
        }
        if(res == NULL){
            return NULL;
        }
        p->next = NULL;
        return res;
    }
};
```

代码写的很复杂，多次判断新建的链表的头指针是否为NULL，能否简化处理呢？



2. 添加空头结点

参考[题解](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/solution/mian-shi-ti-25-he-bing-liang-ge-pai-xu-de-lian-b-2/)。

为合并后链表初始化一个空的头结点，但不用考虑空链表的情况，最后再返回它的next即可。此外，还可以做一个小优化是：**合并剩余链表时，不用再遍历链表，只需改变指针指向即可**，这与有序数组的归并是不同的。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */

 /*
 类似归并排序
 时间复杂度为:O(n)
 空间复杂度为:O(1)
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* res, *p1, *p2, *p;
        p1 = l1;
        p2 = l2;
        res = new ListNode(0);
        p = res;
        while(p1 != NULL && p2 != NULL){//循环合并
            if(p1->val < p2->val){
                p->next = p1;
                p1 = p1->next;
            }
            else{
                p->next = p2;
                p2 = p2->next;
            }
          	p = p->next;
        }
      	//合并剩余链表
        if(p1 != NULL){
            p->next = p1;
        }
        if(p2 != NULL){
            p->next = p2;
        }
        return res->next;
    }
};
```

