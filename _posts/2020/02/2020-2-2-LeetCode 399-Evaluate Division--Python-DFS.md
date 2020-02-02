---
layout: post
title: "LeetCode 399. Evaluate Division--Python-DFS解法-图"
categories: [LeetCode]
description: "LeetCode 399. Evaluate Division--Python-DFS解法"
keywords: LeetCode, DFS
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
题目地址：[Evaluate Division - LeetCode](https://leetcode.com/problems/evaluate-division/)
{% raw %}
***          
{% endraw %}
Equations are given in the format A / B = k, where A and B are variables represented as strings, and k is a real number (floating point number). Given some queries, return the answers. If the answer does not exist, return -1.0.

Example:
Given a / b = 2.0, b / c = 3.0.
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? .
return [6.0, 0.5, -1.0, 1.0, -1.0 ].

The input is: vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries , where equations.size() == values.size(), and the values are positive. This represents the equations. Return vector<double>.

According to the example above:
```
equations = [ ["a", "b"], ["b", "c"] ],
values = [2.0, 3.0],
queries = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ]. 
```
{% raw %}
***          
{% endraw %}
这道题目很好理解，就是有向图的一个权值问题。

解法是从起点开始，使用DFS,或者BFS遍历图，思路跟这道题目很像：[LeetCode 207. Course Schedule--有向图找环--面试算法题--DFS递归，拓扑排序迭代--Python](https://zhang0peter.com/2020/01/29/LeetCode-207-Course-Schedule/)

Python-DFS解法如下：
```python
class Solution:

    def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
        graph = {}
        for (x, y), v in zip(equations, values):
            if x in graph:
                graph[x][y] = v
            else:
                graph[x] = {y: v}
            if y in graph:
                graph[y][x] = 1/v
            else:
                graph[y] = {x: 1/v}

        def dfs(s, t) -> int:
            if s not in graph:
                return -1
            if t == s:
                return 1
            for node in graph[s].keys():
                if node == t:
                    return graph[s][node]
                elif node not in visited:
                    visited.add(node)  # 添加到已访问避免重复遍历
                    v = dfs(node, t)
                    if v != -1:
                        return graph[s][node]*v
            return -1
        res = []
        for qs, qt in queries:
            visited = set()
            res.append(dfs(qs, qt))
        return res

```


