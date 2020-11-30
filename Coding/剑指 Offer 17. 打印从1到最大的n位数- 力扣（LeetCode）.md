# [剑指 Offer 17. 打印从1到最大的n位数- 力扣（LeetCode）](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

## 题目描述

输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

示例 1:

输入: n = 1
输出: [1,2,3,4,5,6,7,8,9]


说明：

用返回一个整数列表来代替打印
n 为正整数

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

1. 简单求解

不考虑大数的问题，n位数最大的数字为$10^n - 1$，将$1 ～ 10^n - 1$的数字添加到数组中即可。

时间复杂度为$O(10^n)$

```c++
class Solution {
public:
    vector<int> printNumbers(int n) {
        vector<int>res;
        int num = pow(10, n) - 1;//n位十进制数最大的一个

        for(int i = 1; i <= num; ++i){
            res.push_back(i);
        }
        return res;
    }
};
```



2. 字符串模拟

考虑大数问题，n的值很大，用字符串来实现增1。

时间复杂度为$O(10^n)$

```c++
/*
用c++模拟大数自增

*/
class Solution {
public:
	vector<int> res;
	vector<int> printNumbers(int n) {
		string str(n, '0');//初始化全0的字符串
        while(addOne(str)){//增1 直到超过n位
            res.push_back(stoi(str));
        }
        return res;
	}

    bool addOne(string& str){//大数增1
        int flag = 0;//进位标记
        bool res = true;
        int sum = 0, len = str.size();
        for(int i = len - 1; i >= 0; --i){
            if(i == len - 1){//自增
                sum = str[i] - '0' + 1;
            }
            else{
                sum = str[i] - '0' + flag;//加上来自低位的进位
            }
            if(sum > 9){
                if(i == 0){//表示结果超过n位,越界
                    res = false;
                }
                else{
                    flag = 1;//有进位
                    str[i] = sum % 10 + '0';
                }
            }
            else{//没有进位，高位保持不变，直接break
                str[i] = sum + '0';
                break;
            }
        }
        return res;
    }
	
};
```

