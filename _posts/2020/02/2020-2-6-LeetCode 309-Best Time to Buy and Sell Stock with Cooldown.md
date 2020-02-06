---
layout: post
title: "LeetCode 309. Best Time to Buy and Sell Stock with Cooldown--Java解法-卖股票系列题目"
categories: [LeetCode]
description: "LeetCode 309. Best Time to Buy and Sell Stock with Cooldown--Java解法-卖股票系列题目"
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
题目地址：[Best Time to Buy and Sell Stock with Cooldown - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)
{% raw %}
***          
{% endraw %}
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)
Example:
```
Input: [1,2,3,0,2]
Output: 3 
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```
{% raw %}
***          
{% endraw %}
这道题目的区别跟之前的买卖股票的题目区别在于卖了之后要停一天。

最容易想到的是贪心，但会发现贪心不能解决问题。

因为后面一个状态跟前面一个状态有关，应该用动态规划解决。

因为之前卖了需要冷却一天，所以需要另外一个变量来存

第n个状态应该与n-1和n-2个状态有关

下面先把解法列出来。

Java 解法如下：
```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 1) {
            return 0;
        }
        /**
         there can be two types of profit we need to track
         sellProf[i] - profit earned by selling on ith day
         restProf[i] - profit earned by resting on ith day
         */
        int sellProf = 0;
        int restProf = 0;
        int lastProf = 0;
        for (int i = 1; i < prices.length; i++) {
            lastProf = sellProf;
            //the current sellProf is either by selling on ith day or by resting on ith day
            sellProf = Math.max(sellProf + prices[i] - prices[i - 1], restProf);
            restProf = Math.max(lastProf, restProf);
        }
        return Math.max(sellProf, restProf);
    }
}
```


下面以`[1, 2, 3, 0, 2]`为例：

{% raw %}
     
|          | 初始值 | 1   | 2   | 3   | 0   | 2   |
| -------- | ------ | --- | --- | --- | --- | --- |
| sellProf | 0      |     | 1   | 2   | 1   | 3   |
| restProf | 0      |     | 0   | 1   | 2   | 2   |
| lastProf | 0      |     | 0   | 1   | 2   | 1   |


{% endraw %}



这里有2个主要的变量，一个是`sellProf`，另一个是`restProf`.

`sellProf`变量意味着当前卖掉股票的总收入或者昨天休息的总收入。这里有个难点，那就是如果你昨天卖了股票，今天又卖股票，实际意味着前天卖的股票，今天卖了。

`restProf`变量意味着昨天卖掉股票，今天继续休息的总收入或者昨天休息的总收入。
