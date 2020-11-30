# [剑指 Offer 30. 包含min函数的栈 - 力扣（LeetCode)](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

## 题目描述

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

 

示例:

MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.


提示：

各函数的调用总次数不超过 20000 次

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

正常调用min函数的时间复杂度为：$O(n)$，为了将复杂度降至$O(1)$，可以采用“空间换时间”的思路，用一个辅助栈去存储当前栈中所有元素的最小元素，调用min函数时，直接返回辅助栈的栈顶元素即可，在`push`和`pop`时，更新辅助栈的元素。

辅助栈的空间大小与原始栈相同，所以空间复杂度为：$O(n)$，时间复杂度为：$O(1)$。

```cpp
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int>sta;
    stack<int>minEle;
    MinStack() {   
    }
    
    void push(int x) {
        if(minEle.empty()){//更新辅助栈
            minEle.push(x);
        }
        else{
            minEle.push(x < minEle.top()?x:minEle.top());
        }
        sta.push(x);
    }
    
    void pop() {
        sta.pop();
        minEle.pop();//弹出辅助栈栈顶元素
    }
    
    int top() {
        return sta.top();
    }
    
    int min() {
        return minEle.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->min();
 */
```

**优化** ，参考[题解](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/solution/mian-shi-ti-30-bao-han-minhan-shu-de-zhan-fu-zhu-z/)，辅助栈只存放**非严格降序元素** （为什么不是严格降序元素，这里要细品），这样可以节省一部分存储空间，只有在最差的情况下，存储元素个数与原始栈相等，一般情况下都是少于原始储，对空间进行了一定程度的优化。

```cpp
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int>sta;
    stack<int>minEle;
    MinStack() {   
    }
    
    void push(int x) {
        if(minEle.empty() || x <= minEle.top()){//存储非严格降序的元素
            minEle.push(x);
        }
        sta.push(x);
    }
    
    void pop() {
        if(sta.top() == minEle.top()){//一致时，才弹辅助栈栈顶
            minEle.pop();
        }
        sta.pop();     
    }
    
    int top() {
        return sta.top();
    }
    
    int min() {
        return minEle.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->min();
 */
```

