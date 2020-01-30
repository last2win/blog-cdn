---
layout: post
title: "LeetCode 123. Best Time to Buy and Sell Stock III--Python解法--动态规划--数学题"
categories: [LeetCode]
description: "LeetCode 123. Best Time to Buy and Sell Stock III--Python解法--动态规划--数学题"
keywords: LeetCode
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}



LeetCode题解文章分类：[LeetCode题解文章集合](https://zhang0peter.com/categories/#LeetCode)               
LeetCode 所有题目总结：[LeetCode 所有题目总结](https://zhang0peter.blog.csdn.net/article/details/100055202)                                  
{% raw %}
***          
{% endraw %}
题目地址：[Best Time to Buy and Sell Stock III - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)
{% raw %}
***          
{% endraw %}
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most two transactions.

Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

Example 1:
```
Input: [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
             Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
```
Example 2:
```
Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.
```
Example 3:
```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```
{% raw %}
***          
{% endraw %}
这道题目乍一看用贪心可以做，可以过题目给的样例，但实际上并不行，考虑样例如下：
```
[1,3,2,7]
```
这道题目看起来跟这道很像：[LeetCode 122. Best Time to Buy and Sell Stock II--贪心--Java,C++,Python解法](https://blog.csdn.net/zhangpeterx/article/details/91126364)，但其实不一样。

感觉应该用动态规划来做，但不好做。

我想不出解法，看了别人的做法，感觉很神奇。

Python解法如下：
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if prices == []:
            return 0
        buyOne = 0x7ffffff
        SellOne = 0
        buyTwo = 0x7ffffff
        SellTwo = 0
        for p in prices:
            buyOne = min(buyOne, p)
            SellOne = max(SellOne, p - buyOne)
            buyTwo = min(buyTwo, p - SellOne)
            SellTwo = max(SellTwo, p - buyTwo)
        return SellTwo

```
下面以`[1, 3, 2, 7]`为例：

{% raw %}
     
|         | 初始值    | 1   | 3   | 2   | 7   |
| ------- | --------- | --- | --- | --- | --- |
| buyOne  | 0x7ffffff | 1   | 1   | 1   | 1   |
| SellOne | 0         | 0   | 2   | 2   | 6   |
| buyTwo  | 0x7ffffff | 1   | 1   | 0   | 0   |
| SellTwo | 0         | 0   | 2   | 2   | 7   |


{% endraw %}


可以看出`buyOne`变量代表的是全局最小值,`SellOne`变量代表的是只买卖一次股票的全局的最大获利

`buyTwo`变量表示的是用获利的钱(只买卖一次的全局最多的钱)购买当天股票后剩余的钱，`SellTwo`变量代表买卖股票2次获利最多的钱。

解法真的很神奇。