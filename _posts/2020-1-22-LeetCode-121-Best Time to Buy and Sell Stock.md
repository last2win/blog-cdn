---
layout: post
title: "LeetCode 121. Best Time to Buy and Sell Stock--Java，Python，C++解法"
categories: [LeetCode]
description: "LeetCode 121. Best Time to Buy and Sell Stock--Java，Python，C++解法"
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
题目地址：[Best Time to Buy and Sell Stock - LeetCode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
{% raw %}
***          
{% endraw %}
Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

Example 1:
```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```
Example 2:
```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```
{% raw %}
***          
{% endraw %}
这道题目遍历一遍即可。
Python解法如下：
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        maxprofit=0
        minprice=-1
        for i in prices:
            if minprice==-1:
                minprice=i
            elif i<minprice:
                minprice=i
            elif i-minprice>maxprofit:
                maxprofit=i-minprice
        return maxprofit
```
Java解法如下：
```java
public class Solution {
    public int maxProfit(int prices[]) {
        int minprice = Integer.MAX_VALUE;
        int maxprofit = 0;
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < minprice)
                minprice = prices[i];
            else if (prices[i] - minprice > maxprofit)
                maxprofit = prices[i] - minprice;
        }
        return maxprofit;
    }
}
```
C++解法如下：
```c
class Solution {
public:
int maxProfit(vector<int> &prices) {
    int maxPro = 0;
    int minPrice = INT_MAX;
    for(int i = 0; i < prices.size(); i++){
        minPrice = min(minPrice, prices[i]);
        maxPro = max(maxPro, prices[i] - minPrice);
    }
    return maxPro;
}
};
```
