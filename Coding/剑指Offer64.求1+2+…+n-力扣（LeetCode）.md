# [剑指 Offer 64. 求1+2+…+n - 力扣（LeetCode）](https://leetcode-cn.com/problems/qiu-12n-lcof/)

## 题目描述

求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

 

示例 1：

输入: n = 3
输出: 6
示例 2：

输入: n = 9
输出: 45

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/qiu-12n-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

使用**高斯公式** $n*(n + 1) / 2$，会用到乘除法，不符合题意。

使用循环求和，则会使用循环语句，不符合题意。

而循环可以用递归来代替。

```cpp
class Solution {
public:
    int sumNums(int n) {
        if(n == 0) return 0;
        else return n + sumNums(n - 1);
    }
};
```

但是这样写递归出口的判断就会用到条件判断语句，不符合题意。

可以利用 **逻辑语句的短路特性**来实现条件判断，当`n= 0`时，后面语句就不会执行，成功实现了退出递归。

```cpp
//不能用循环 就采用递归
//递归的出口判断需要逻辑符的短路特性
class Solution {
public:
    int sumNums(int n) {
        bool x = n && (n += sumNums(n - 1));
        return n;
    }
};
```

