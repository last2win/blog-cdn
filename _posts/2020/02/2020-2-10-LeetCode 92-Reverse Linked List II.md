---
layout: post
title: "LeetCode 92. Reverse Linked List II--Python 解法--反转部分链表--笔试算法题"
categories: [LeetCode]
description: "LeetCode 92. Reverse Linked List II--Python 解法--反转部分链表--笔试算法题"
keywords: LeetCode
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
题目地址：[Reverse Linked List II - LeetCode](https://leetcode.com/problems/reverse-linked-list-ii/)
{% raw %}
***          
{% endraw %}
Reverse a linked list from position m to n. Do it in one-pass.

Note: 1 ≤ m ≤ n ≤ length of list.

Example:
```
Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL
```

{% raw %}
***          
{% endraw %}
这道题目是我之前做一家公司笔试时候遇到的算法题，反转部分链表，难度比反转全部链表大。

当时做题的时候不会告诉你错误的样例，现在重新写，在特殊情况的判断中依然会有出错，以后需要重新再做。

Python解法如下：
```python
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None


class Solution:
    def reverseBetween(self, head: ListNode, m: int, n: int) -> ListNode:
        def helper(reverse_head, count):
            if reverse_head is None:
                return reverse_head
            curr = reverse_head
            prev = None
            for _ in range(0, count):
                next_node = curr.next
                curr.next = prev
                prev = curr
                curr = next_node
            reverse_head.next = curr
            return prev

        if n - m == 0:
            return head
        reverse_before_head = head
        if m == 1:
            reverse_head = helper(reverse_before_head, n - m + 1)
            return reverse_head
        for _ in range(1, m - 1):
            reverse_before_head = reverse_before_head.next
        reverse_head = helper(reverse_before_head.next, n - m + 1)
        reverse_before_head.next = reverse_head
        return head
```

时间复杂度为O(n)，空间复杂度为O(1)。

