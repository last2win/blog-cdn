---
layout: post
title: "LeetCode 31. Next Permutation-- Python 解法--数学题--比当前数大的最小的数"
categories: [LeetCode]
description: "LeetCode 31. Next Permutation-- Python 解法--数学题--比当前数大的最小的数"
keywords: LeetCode, Python
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}


LeetCode 31. Next Permutation-- Python 解法--数学题--比当前数大的最小的数

LeetCode题解文章分类：[LeetCode题解文章集合](https://zhang0peter.com/categories/#LeetCode)               
LeetCode 所有题目总结：[LeetCode 所有题目总结](https://zhang0peter.blog.csdn.net/article/details/100055202)                                  
{% raw %}
***          
{% endraw %}
题目地址：[Next Permutation - LeetCode](https://leetcode.com/problems/next-permutation/)
{% raw %}
***          
{% endraw %}
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

{% raw %}
***          
{% endraw %}

这道题目一看就知道是经典的数学题，而且根据统计是高频面试算法题，`Facebook`尤为喜欢这题。

这道题目看起来简单，做起来难度还是挺大的。

1.先考虑特殊情况，如果这个数是完全倒叙排序的，也就是最大的，那么只要返回正序排序最小的即可。

2.想要实现比当前数大的最小的数，改变低位，不要修改高位。

3.如果一个数的低位部分是倒叙排序，然后高位不是倒序排序，那么交换高位和低位中大于它的这个数，然后把低位顺序排序即可。

4.这道题目另外一个需要注意的难点是需要就地排序，不能把指针引用指向新的数组。

Python解法如下：
```python
class Solution:
    def nextPermutation(self, nums) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        flag = -1
        for i in range(len(nums)-2, -1, -1):
            if nums[i] < nums[i+1]:
                flag = i
                break
        if flag == -1:
            nums.sort()
        else:
            for i in range(len(nums)-1, flag, -1):
                if nums[i] > nums[flag]:
                    nums[i], nums[flag] = nums[flag], nums[i]
                    nums[flag+1:] = sorted(nums[flag+1:])
                    break

```

时间复杂度为O(n)，空间复杂度为O(1)。

