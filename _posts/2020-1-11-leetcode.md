---
layout: post
title: LeetCode 832 	 Flipping an Image
categories: [LeetCode]
description: LeetCode 832 	 Flipping an Image 题解
keywords: 
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         
{% raw %}
***          
{% endraw %}



LeetCode题解专栏：[LeetCode题解](https://blog.csdn.net/zhangpeterx/article/category/8722334)              
LeetCode 所有题目总结：[LeetCode 所有题目总结](https://blog.csdn.net/zhangpeterx/article/details/100055202)               
大部分题目C++，Python，Java的解法都有。                
{% raw %}
***          
{% endraw %}
题目地址：[Flipping an Image - LeetCode](https://leetcode.com/problems/flipping-an-image/)
{% raw %}
***          
{% endraw %}
Given a binary matrix A, we want to flip the image horizontally, then invert it, and return the resulting image.

To flip an image horizontally means that each row of the image is reversed.  For example, flipping [1, 1, 0] horizontally results in [0, 1, 1].

To invert an image means that each 0 is replaced by 1, and each 1 is replaced by 0. For example, inverting [0, 1, 1] results in [1, 0, 0].

Example 1:
```
Input: [[1,1,0],[1,0,1],[0,0,0]]
Output: [[1,0,0],[0,1,0],[1,1,1]]
Explanation: First reverse each row: [[0,1,1],[1,0,1],[0,0,0]].
Then, invert the image: [[1,0,0],[0,1,0],[1,1,1]]
```
Example 2:
```
Input: [[1,1,0,0],[1,0,0,1],[0,1,1,1],[1,0,1,0]]
Output: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
Explanation: First reverse each row: [[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]].
Then invert the image: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
```
Notes:

* 1 <= A.length = A[0].length <= 20
* 0 <= A[i][j] <= 1
{% raw %}
***          
{% endraw %}

题目的意思是给定二进制矩阵A，我们水平翻转图像，然后反转它，并返回结果图像。

java代码如下：
```java
class Solution {
    public int[][] flipAndInvertImage(int[][] A) {
        for (int i = 0; i < A.length; i++) {
            for (int j = 0; j < A[i].length / 2; j++) {
                int temp = A[i][j];
                A[i][j] = A[i][A.length - 1 - j];
                A[i][A.length - 1 - j] = temp;
            }
        }

        for (int i = 0; i < A.length; i++) {
            for (int j = 0; j < A[0].length; j++) {
                A[i][j] = A[i][j] == 0 ? 1 : 0;
            }
        }
        return A;
    }
}
```
Python的解法基本一致，就不贴了。