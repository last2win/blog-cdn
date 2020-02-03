---
layout: post
title: "LeetCode 41. First Missing Positive--Python 解法--数学题-找到不存在的最小正整数-O(1)空间复杂度"
categories: [LeetCode]
description: "LeetCode 41. First Missing Positive--Python 解法--数学题-找到不存在的最小正整数"
keywords: LeetCode, Python
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}








LeetCode题解文章分类：[LeetCode题解文章集合](https://zhang0peter.com/categories/#LeetCode)               
LeetCode 所有题目总结：[LeetCode 所有题目总结](https://zhang0peter.blog.csdn.net/article/details/100055202)                                  
{% raw %}
***          
{% endraw %}


题目地址：[First Missing Positive - LeetCode](https://leetcode.com/problems/first-missing-positive/)
{% raw %}
***          
{% endraw %}


Given an unsorted integer array, find the smallest missing positive integer.

Example 1:
```
Input: [1,2,0]
Output: 3
```
Example 2:
```
Input: [3,4,-1,1]
Output: 2
```
Example 3:
```
Input: [7,8,9,11,12]
Output: 1
```
Note:

Your algorithm should run in O(n) time and uses constant extra space.


{% raw %}
***          
{% endraw %}



这道题目跟这道很像：[LeetCode 31. Next Permutation-- Python 解法--数学题--比当前数大的最小的数](https://zhang0peter.com/2020/01/22/LeetCode-31-Next-Permutation/)

但这道题目难度明显更大，因为没有明确数组大小，而且有负数。

首先，时间复杂度至少为O(N)，因为要遍历数组一遍，空间复杂度可大可小，但O(N)的复杂度是可以接受的。

首先确定数组长度，最小缺失的正整数最大为N+1

遍历数组，使用bitmap确认存这个数是否存在过

Python解法如下：
```python
class Solution:
    def firstMissingPositive(self, nums) -> int:
        length = len(nums)
        l = [0]*(length+2)
        for i in nums:
            if 0 < i < 1 + length:
                l[i] = 1
        for i in range(1, length+3):
            if l[i] == 0:
                return i
```
此解法时间复杂度为O(n)，空间复杂度为O(n)。

想要空间复杂度降为O(1)是可以做到的，但不一定会更快，因为CPU缓存的原因，到处交换数组不一定快：
```python
class Solution:
    def firstMissingPositive(self, nums) -> int:
        end = len(nums)
        i = 0
        while i != end:
            val = nums[i]
            if val == i+1:
                i += 1
            elif val > end or val <= 0 or nums[val-1] == val:
                end -= 1
                nums[i], nums[end] = nums[end], nums[i]
            else:
                nums[i], nums[val-1] = nums[val-1], nums[i]
        return i+1

```
