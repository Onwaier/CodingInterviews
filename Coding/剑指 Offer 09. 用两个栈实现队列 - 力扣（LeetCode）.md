# [剑指 Offer 09. 用两个栈实现队列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

## 题目描述

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

 

示例 1：

输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
示例 2：

输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
提示：

1 <= values <= 10000
最多会对 appendTail、deleteHead 进行 10000 次调用

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

1. 两个栈模拟队列

栈1用来入队，栈2用来出队，当栈2为空，栈1不为空时，将栈1所有的元素压入栈2中。当栈1与栈2都为空，则无元素可以出栈，直接返回-1.



```c++
/*
两个栈用来队列
栈1用来入队，栈2用来出队
*/

class CQueue {
public:
    //sta1用来入队， sta2用来出队
    stack<int>sta1, sta2;
    CQueue() {
    }
    
    void appendTail(int value) {
        sta1.push(value);//入队
    }
    
    int deleteHead() {
        int res;
        if(sta2.empty()){//当栈2为空时，需要将栈1中的所有元素压入到栈2
            while(!sta1.empty()){
                sta2.push(sta1.top());
                sta1.pop();
            }
        }
        if(!sta2.empty()){//
            res = sta2.top();
            sta2.pop();
        }
        else{//两个栈都为空，无元素出队，直接返回-1
            res = -1;
        }
        return res;
    }
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
 * obj->appendTail(value);
 * int param_2 = obj->deleteHead();
 */
```

