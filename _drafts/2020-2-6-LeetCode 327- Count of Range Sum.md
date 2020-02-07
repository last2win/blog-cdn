---
layout: post
title: "LeetCode 327. Count of Range Sum--Java, Python 解法"
categories: [LeetCode]
description: ""
keywords: LeetCode
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}







此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         
***
LeetCode题解文章分类：[LeetCode题解文章集合](https://zhang0peter.com/categories/#LeetCode)               
LeetCode 所有题目总结：[LeetCode 所有题目总结](https://zhang0peter.blog.csdn.net/article/details/100055202)                                  
***
题目地址：[Count of Range Sum - LeetCode](https://leetcode.com/problems/count-of-range-sum/)
***

Given an integer array nums, return the number of range sums that lie in [lower, upper] inclusive.
Range sum S(i, j) is defined as the sum of the elements in nums between indices i and j (i ≤ j), inclusive.

Note:
A naive algorithm of O(n2) is trivial. You MUST do better than that.

Example:
```
Input: nums = [-2,5,-1], lower = -2, upper = 2,
Output: 3 
Explanation: The three ranges are : [0,0], [2,2], [0,2] and their respective sums are: -2, -1, 2.
```
***

这道题目的意思是求数组中所有组合的和的可能性。

最容易想到的解法是暴力，但时间复杂度明显太高。

对于专门存储区间和的数据结构，很容易就想到线段树(Segment Tree)，线段树的详细介绍见我写的另一篇博客：[数据结构线段树介绍与笔试算法题-LeetCode 307. Range Sum Query - Mutable--Java解法](https://zhang0peter.com/2020/01/28/LeetCode-307-Range-Sum-Query-Mutable-Java/)


穷举的Java解法如下：
```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            long res = 0;
            for (int j = i; j < nums.length; j++) {
                res += nums[j];
                if (res >= lower && res <= upper) {
                    count += 1;
                }
            }
        }
        return count;
    }
}
```
花费127ms

使用线段树的解法如下：
```java
class SegmentTree {

    int[] segmentTree;
    int n;

    public SegmentTree(int[] nums) {
        if (nums.length > 0) {
            n = nums.length;
            segmentTree = new int[n * 2];
            //把原数组拷贝到新数组的后半部分
            System.arraycopy(nums, 0, segmentTree, 0 + n, n);
            //计算区域和
            for (int i = n - 1; i > 0; i--) {
                segmentTree[i] = segmentTree[i * 2] + segmentTree[i * 2 + 1];
            }
        }
    }

    public long sumRange(int left, int right) {
        left += n;
        right += n;
        long sum = 0;
        while (left <= right) {
            //判断左索引节点是否是父节点的右子节点
            if ((left % 2) == 1) {
                sum += segmentTree[left];
                left++;
            }
            //判断右索引节点是否是父节点的左子节点
            if ((right % 2) == 0) {
                sum += segmentTree[right];
                right--;
            }
            left /= 2;
            right /= 2;
        }
        return sum;
    }
}

class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        SegmentTree segmentTree = new SegmentTree(nums);
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            for (int j = i; j < nums.length; j++) {
                long res = segmentTree.sumRange(i, j);
                if (res >= lower && res <= upper) {
                    count += 1;
                }
            }
        }
        return count;
    }
}
```
居然超时了，比穷举还慢。。。。。

随后我看到了讨论：[Share my solution - LeetCode Discuss](https://leetcode.com/problems/count-of-range-sum/discuss/77990/Share-my-solution)

顿时让我想起了之前做的一道题目：[LeetCode 974. Subarray Sums Divisible by K--Python解法--数学题--取模求余](https://zhang0peter.blog.csdn.net/article/details/103095241)




