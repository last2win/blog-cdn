---
layout: post
title: "LeetCode 207. Course Schedule--有向图找环--面试算法题--DFS递归，拓扑排序迭代--Python"
categories: [LeetCode, 数据结构]
description: "LeetCode 207. Course Schedule--有向图找环--面试算法题--DFS递归，拓扑排序迭代--Python-DAG"
keywords: LeetCode, DAG, python, circle
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
题目地址：[Course Schedule - LeetCode](https://leetcode.com/problems/course-schedule/)

{% raw %}
***          
{% endraw %}
There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

Example 1:
```
Input: 2, [[1,0]] 
Output: true
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0. So it is possible.
```
Example 2:
```
Input: 2, [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible.
```
Note:

The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
You may assume that there are no duplicate edges in the input prerequisites.

{% raw %}
***          
{% endraw %}
这道题目是我在XX公司面试时遇到的算法题，当时就看出来这是判断有向图（DAG）是否存在环。

一时想不起算法，于是就自己实现了一个时间复杂度为O(V^2)的算法，面试官自然不满意，于是当时现场又用递归的做法实现了时间复杂度为O(N+V)的算法(dfs)，这个做法只是口述了一遍，并没有真的实现。现在想想实现起来还是非常复杂的。


刚刚我实现了O(V^2)的算法，发现算法有bug，尴尬了。

然后实现自己的DFS算法，花了至少20分钟，现场做肯定做不出来。

DFS-Python解法如下：
```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        s = {}
        res = set()
        flag = 0

        def helper(begin=-1, find=set()):
            nonlocal flag, res, s
            for i in s[begin]:
                if i not in res:
                    if i in find:
                        flag = 1
                        return
                    find.add(i)
                    helper(i, find=find)
            res.add(begin)

        for i in range(0, numCourses):
            s[i] = set()
        for i in prerequisites:
            s[i[0]].add(i[1])
        for i in range(0, numCourses):
            helper(begin=i, find={i})
            if flag == 1:
                return False
        return True
```
代码不好，别人写的优化过的代码如下：
```python
class Solution(object):
    def canFinish(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: bool
        """
        graph = [[] for _ in range(numCourses)]
        visited = [0 for _ in range(numCourses)]
        # create graph
        for pair in prerequisites:
            x, y = pair
            graph[x].append(y)
        # visit each node
        for i in range(numCourses):
            if not self.dfs(graph, visited, i):
                return False
        return True
    
    def dfs(self, graph, visited, i):
        # if ith node is marked as being visited, then a cycle is found
        if visited[i] == -1:
            return False
        # if it is done visted, then do not visit again
        if visited[i] == 1:
            return True
        # mark as being visited
        visited[i] = -1
        # visit all the neighbours
        for j in graph[i]:
            if not self.dfs(graph, visited, j):
                return False
        # after visit all the neighbours, mark it as done visited
        visited[i] = 1
        return True
```

随后看到另外一位大佬用OOP的思想重写了代码，虽然速度没有提升，但思路清晰多了：
```python
class Solution:
    class Course(object):
        def __init__(self):
            self.being_visit, self.visit_done = False, False
            self.pre_course = []

        def is_cyclic(self):
            if self.visit_done is True:
                return False
            if self.being_visit is True:
                return True
            self.being_visit = True
            for course in self.pre_course:
                if course.is_cyclic():
                    return True
            self.being_visit = False
            self.visit_done = True
            return False

    def canFinish(self, num_courses, prerequisites) -> bool:
        l = [self.Course() for _ in range(0, num_courses)]
        for (course1, course2) in prerequisites:
            l[course1].pre_course.append(l[course2])
        for i in range(0, num_courses):
            if l[i].is_cyclic():
                return False
        return True
```

标准的更正式更快的解法是`拓扑排序（Topological Sorting）`，可以参考：[Kahn's algorithm for Topological Sorting - GeeksforGeeks](https://www.geeksforgeeks.org/topological-sorting-indegree-based-solution/)

```python
class Solution:
    def canFinish(self, num_courses, prerequisites) -> bool:
        vertices = num_courses
        adj_list = [[] for _ in range(0, vertices)]
        in_degree = [0 for _ in range(0, vertices)]
        for (course1, course2) in prerequisites:
            adj_list[course1].append(course2)
            in_degree[course2] += 1
        q = [i for i in range(0, vertices) if in_degree[i] == 0]
        count = 0
        while q:
            front = q.pop(0)
            for i in adj_list[front]:
                in_degree[i] -= 1
                if in_degree[i] < 0:
                    return False
                elif in_degree[i] == 0:
                    q.append(i)
            count += 1
        if count == vertices:
            return True
        else:
            return False
```


