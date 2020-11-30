# [剑指 Offer 31. 栈的压入、弹出序列  - 力扣（LeetCode）](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

## 题目描述

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

 

示例 1：

输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
示例 2：

输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。


提示：

0 <= pushed.length == popped.length <= 1000
0 <= pushed[i], popped[i] < 1000
pushed 是 popped 的排列。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

模拟法，尝试构造与出栈序列一致的出栈顺序，如果元素已全部入栈，栈顶却不能匹配出栈序列的当前元素，则表示无法构造，返回false.



该算法依赖于“入栈数字都不相等”这一限制。

时间复杂度为：$O(n)$，所有元素均入栈一次，然后再出栈。所以时间复杂度为：$O(n)$。

空间复杂度为：$O(n)$，最坏的情况是元素全部入栈再全部出栈，此时辅助栈的大小与原始序列长度相等。

```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int>sta;
        int idx1 = 0, idx2 = 0, len = pushed.size();
        while(idx2 < len){
            while(sta.empty() || sta.top() != popped[idx2]){
                if(idx1 >= len){//元素已全部入栈，但与出栈序列不匹配
                    return false;
                }
                sta.push(pushed[idx1++]);//入栈
            }
            sta.pop();//按照出栈序列出栈
            ++idx2;
        }
        return true;
    }
};
```

