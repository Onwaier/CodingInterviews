# [剑指 Offer 41*. 数据流中的中位数 - LeetCode（力扣）](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

## 题目描述

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

void addNum(int num) - 从数据流中添加一个整数到数据结构中。
double findMedian() - 返回目前所有元素的中位数。
示例 1：

输入：
["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
[[],[1],[2],[],[3],[]]
输出：[null,null,null,1.50000,null,2.00000]
示例 2：

输入：
["MedianFinder","addNum","findMedian","addNum","findMedian"]
[[],[2],[],[3],[]]
输出：[null,null,2.00000,null,2.50000]


限制：

最多会对 addNum、findMedian 进行 50000 次调用。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 题解

参考[题解](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/solution/mian-shi-ti-41-shu-ju-liu-zhong-de-zhong-wei-shu-y/)，这个题的解题思路很巧妙，充分利用大根堆与小根堆的特点。

具体思路：维护大根堆（存放较小的一部分元素）和小根堆（存放较大的一部分），两堆间元素个数差不会超过1。

添加元素时，当两堆元素不等时，将新元素添加到大根堆（但并不是直接添加），先将元素添加小根堆，然后将小根堆的堆顶元素添加到大根堆中（这样就保证大根堆中存放的元素为较小的一部分。）当两堆元素相等时，将新元素添加到小根堆（**添加方式类似将元素添加到小根堆**），时间复杂度为：$O(logn)$。

查询中位数时，当两堆元素不等时，即总个数为奇数，返回小根堆的堆顶元素 ；当两堆元素相等时，即总个数为偶数，返回两堆堆顶元素和的平均数，时间复杂度为：$O(1)$。



```cpp
class MedianFinder {
public:
    /** initialize your data structure here. */
    priority_queue<int, vector<int>, greater<int>>smallHeap;//小根堆 存放较大的一半
    priority_queue<int, vector<int>, less<int>>bigHeap;//大根堆  存放较小的一半
    MedianFinder() {

    }
    
    void addNum(int num) {
        if(smallHeap.size() == bigHeap.size()){//相等时，将元素添加到小根堆
            //具体过程为先将元素添加到大根堆，然后再将大根堆的堆顶元素添加小根堆
            bigHeap.push(num);
            int topNum = bigHeap.top();
            bigHeap.pop();
            smallHeap.push(topNum);
        }
        else{//将元素添加到大根堆
            //具体过程为先将元素添加到小根堆，然后再将小根堆的堆顶元素添加大根堆
            smallHeap.push(num);
            int topNum = smallHeap.top();
            smallHeap.pop();
            bigHeap.push(topNum);
        }
    }
    
    double findMedian() {
        if(smallHeap.size() == bigHeap.size()){//此时中位数为 小根堆与大根堆堆顶元素的平均数
            return (smallHeap.top() + bigHeap.top()) / 2.0;
        }
        else{
            return smallHeap.top();
        }
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```



另一种常见的思路，维持一个有序数组，添加元素至有序数组的时间复杂度为$O(n)$（二分查找插入位置的时间复杂度为$O(logn)$，插入元素的复杂度为$O(n)$，总的复杂度就为$O(n)$），查询中位数的复杂度也为$O(1)$。

```cpp
class MedianFinder {
public:
    /** initialize your data structure here. */
    vector<int>arr;//维护元素队列
    MedianFinder() {

    }
    
    void addNum(int num) {
        int idx = 0;
        if(arr.size() == 0){
            arr.push_back(num);
        }
        else{
            //查找第一个大于num的位置  并插入元素
            arr.insert(upper_bound(arr.begin(), arr.end(), num), num);
        }
    }
    
    double findMedian() {
        if(arr.size() & 1 == 1){//奇数
            return arr[arr.size() / 2];
        }
        else{
            int num1 = arr.size() / 2, num2 = num1 - 1;
            return (arr[num1] + arr[num2]) / 2.0;
        }
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

