# [剑指 Offer 52. 两个链表的第一个公共节点 - 力扣（LeetCode）](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

## 题目描述

输入两个链表，找出它们的第一个公共节点。

如下面的两个链表：



在节点 c1 开始相交。

 

示例 1：

![img](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。




示例 2：

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png)

输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。




示例 3：

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)

输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。


注意：

* 如果两个链表没有交点，返回 null.
* 在返回结果后，两个链表仍须保持原有的结构。
* 可假定整个链表结构中没有循环。
* 程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

**双指针**

思路：

1. 先分别遍历两条链表，时间复杂度为$O(m + n)$。求出两条链表的长度，进而求出长度差diff。
2. 较短的链表，先移动diff个长度的指针。时间复杂度为：$O(abs(n-m))$
3. 同时移动两个指针，直到它们相遇。

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
 找到两条链表的长度差dif
 然后长的指针链表提前移动dif
 然后同时移动，指向的相同结点即为第一个相交结点
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int len1 = 0, len2 = 0;
        ListNode *p1, *p2, *res = NULL;
        p1 = headA;
        p2 = headB;

        //求两个链表的长度
        while(p1 != NULL){
            ++len1;
            p1 = p1->next;
        }
        while(p2 != NULL){
            ++len2;
            p2 = p2->next;
        }

        //指针复位
        p1 = headA;
        p2 = headB;

        int cnt = abs(len1 - len2);
        if(len1 > len2){
            while(cnt--){
                p1 = p1->next;
            }
        }
        if(len2 > len1){
            while(cnt--){
                p2 = p2->next;
            }
        }

        //p1 p2同时移动
        while(p1 != NULL && p2 != NULL){
            if(p1 == p2){
                res = p1;
                break;
            }
            p1 = p1->next;
            p2 = p2->next;
        }
        return res;
    }
};
```

**优美的双指针**

参考[题解](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/solution/shuang-zhi-zhen-fa-lang-man-xiang-yu-by-ml-zimingm/)

改进的思路：

1. 同时移动双指针，当短链表的指针指向链表结尾时，将其指针重新指向长链表的头部。继续同时移动双指针，当长链表的指针指向链表结尾时，将其指针重新指向短链表的头部。时间复杂度为：$O(max(m, n))$
2. 同时移动双指针，直到两指针相遇。

改进思路的第1步相当于原来的1-2步，一定程度上节省了时间。

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *p1 = headA;
        ListNode *p2 = headB;
        
        while (p1 != p2) {
            p1 = p1 != NULL ? p1->next : headB;
            p2 = p2 != NULL ? p2->next : headA;
        }
        return p1;
    }
};
```

