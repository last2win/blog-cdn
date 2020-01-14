---
layout: post
title: LeetCode 1117. Building H2O --Java解法--多线程保证执行顺序--AtomicInteger 
categories: [LeetCode]
description: 使用 AtomicInteger 保证原子性和有序性
keywords: LeetCode, AtomicInteger, Java
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         
{% raw %}
***          
{% endraw %}

LeetCode题解文章分类：[LeetCode题解](https://zhang0peter.com/categories/#LeetCode)               
LeetCode 所有题目总结：[LeetCode 所有题目总结](https://blog.csdn.net/zhangpeterx/article/details/100055202)                                  
{% raw %}
***          
{% endraw %}
题目地址：[Building H2O - LeetCode](https://leetcode.com/problems/building-h2o/)
{% raw %}
***          
{% endraw %}
There are two kinds of threads, oxygen and hydrogen. Your goal is to group these threads to form water molecules. There is a barrier where each thread has to wait until a complete molecule can be formed. Hydrogen and oxygen threads will be given releaseHydrogen and releaseOxygen methods respectively, which will allow them to pass the barrier. These threads should pass the barrier in groups of three, and they must be able to immediately bond with each other to form a water molecule. You must guarantee that all the threads from one molecule bond before any other threads from the next molecule do.

In other words:

If an oxygen thread arrives at the barrier when no hydrogen threads are present, it has to wait for two hydrogen threads.
If a hydrogen thread arrives at the barrier when no other threads are present, it has to wait for an oxygen thread and another hydrogen thread.
We don’t have to worry about matching the threads up explicitly; that is, the threads do not necessarily know which other threads they are paired up with. The key is just that threads pass the barrier in complete sets; thus, if we examine the sequence of threads that bond and divide them into groups of three, each group should contain one oxygen and two hydrogen threads.

Write synchronization code for oxygen and hydrogen molecules that enforces these constraints.

 

Example 1:
```
Input: "HOH"
Output: "HHO"
Explanation: "HOH" and "OHH" are also valid answers.
```
Example 2:
```
Input: "OOHHHH"
Output: "HHOHHO"
Explanation: "HOHHHO", "OHHHHO", "HHOHOH", "HOHHOH", "OHHHOH", "HHOOHH", "HOHOHH" and "OHHOHH" are also valid answers.
 
```
Constraints:

Total length of input string will be 3n, where 1 ≤ n ≤ 20.
Total number of H will be 2n in the input string.
Total number of O will be n in the input string.

{% raw %}
***          
{% endraw %}
这道题目的意思是多线程情况下如何保证代码执行的顺序，是一道经典的多线程的题目。

比较容易想到使用信号量：Semaphore，但问题的难点在于怎样使用信号量保证一致性。
经过思考，我觉得没必要使用信号量，使用 AtomicInteger 就够用了，最终代码如下。

Java解法如下：
```java
class H2O {
    private final AtomicInteger h2o = new AtomicInteger();

    public H2O() {

    }

    public void hydrogen(Runnable releaseHydrogen) throws InterruptedException {
        synchronized (h2o) {
            // if already release two Hydrogen, wait for Oxygen release
            while (h2o.get() == 2) {
                h2o.wait();
            }
            h2o.addAndGet(1);
            // releaseHydrogen.run() outputs "H". Do not change or remove this line.
            releaseHydrogen.run();
            h2o.notifyAll();
        }
    }

    public void oxygen(Runnable releaseOxygen) throws InterruptedException {
        synchronized (h2o) {
            // if already release Oxygen, wait for Hydrogen release
            while (h2o.get() < 0) {
                h2o.wait();
            }
            h2o.addAndGet(-2);
            // releaseOxygen.run() outputs "O". Do not change or remove this line.
            releaseOxygen.run();
            h2o.notifyAll();
        }
    }
```

