# [剑指 Offer 40. 最小的k个数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

## 题目描述

输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

 

示例 1：

输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
示例 2：

输入：arr = [0,1,2,1], k = 1
输出：[0]


限制：

0 <= k <= arr.length <= 10000
0 <= arr[i] <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

1. 排序

常见思路是**排序**，然后返回前k个元素（**k个数有序**）。时间复杂度为$O(nlogn)$，效率是比较低的。

代码略。



2. partition寻找第k-1小的数

主要采用的是快排的思想，直到找到第k-1小的数，此时的前k个数（**k个数无序**）即为所求。

时间复杂度为：$O(n) + O(n / 2) + \cdots + O(1)$接近$O(2n)$，最差的情况就是$O(n^2)$

```cpp
/*
时间复杂度为：O(n)
空间复杂度为：O(n)
*/
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if(k == 0 || arr.size() == 0){
            return vector<int>();
        }
        quickSearch(arr, 0, arr.size() - 1, k - 1);
        // partition(arr, 0, arr.size() - 1);
        return vector<int>(arr.begin(), arr.begin() + k);
    }

    void quickSearch(vector<int>&arr, int left, int right, int k){
        int idx = partition(arr, left, right);
        if(idx == k){
            return;
        }
        else if(idx > k){
            quickSearch(arr, left, idx - 1, k);
        }
        else{
            quickSearch(arr, idx + 1, right, k);
        }
    }

    int partition(vector<int>&arr, int left, int right){//快排的每一趟  时间复杂度为O(right - left + 1)
        int tmp = arr[left];
        int i = left, j = right;
        while(i < j){
            while(i < j && arr[j] >= tmp) --j;
            while(i < j && arr[i] <= tmp) ++i;
            if(i < j)
                swap(arr[i], arr[j]);
        }
        swap(arr[left], arr[j]);
        return j;
    }
};
```

3. 小根堆/大根堆

构建小根堆，每次pop堆顶元素，pop_back()，再重新构建小根堆，直到pop k个元素为止。

```cpp
/*
时间复杂度为：O(klogn)
*/
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if(k == 0 || arr.size() == 0){
            return vector<int>();
        }
        vector<int>res;
        make_heap(arr.begin(), arr.end(), greater<int>());
        for(int i = 0; i < k; ++i){
            pop_heap(arr.begin(), arr.end(), greater<int>());
            res.push_back(arr.back());
            arr.pop_back();
        }
        return res;
    }
};
```

每次构建小根堆复杂度为O(logn)，总的时间复杂度为：O(klogn)



维持一个k元素的大根堆，扫描原始数组，如果堆元素个数小于k就直接添加到堆中，如果堆元素个数等于k，就比较堆顶元素是否大于当前元素，是的话就出堆，然后当前元素出堆，反之，就跳过。

```cpp
/*
时间复杂度为：O(nlogk)
*/
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if(k == 0 || arr.size() == 0){
            return vector<int>();
        }
        vector<int>res;
        int len = arr.size();
        for(int i = 0; i < len; ++i){
            if(res.size() < k){
                res.push_back(arr[i]);
                if(res.size() == k){
                    make_heap(res.begin(), res.end(), less<int>());
                }
            }
            else{
                if(res[0] > arr[i]){
                    pop_heap(res.begin(), res.end());
                    res.pop_back();
                    res.push_back(arr[i]);
                    push_heap(res.begin(), res.end(), less<int>()); 
                }
            }
            
        }
        return res;
    }
};
```

