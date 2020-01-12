---
layout: post
title: LeetCode 1115. Print FooBar Alternately--多线程并发问题--Java解法--CyclicBarrier, synchronized, Semaphore 信号量
categories: [LeetCode]
description: Java多种解法保证线程的执行顺序一致
keywords: LeetCode, CyclicBarrier, Semaphore, synchronized
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         
{% raw %}
***          
{% endraw %}


LeetCode题解专栏：[LeetCode题解](https://zhang0peter.com/categories/#LeetCode)               
LeetCode 所有题目总结：[LeetCode 所有题目总结](https://blog.csdn.net/zhangpeterx/article/details/100055202)                  
{% raw %}
***          
{% endraw %}
题目地址：[Print FooBar Alternately - LeetCode](https://leetcode.com/problems/print-foobar-alternately/)
{% raw %}
***          
{% endraw %}
Suppose you are given the following code:
```
class FooBar {
  public void foo() {
    for (int i = 0; i < n; i++) {
      print("foo");
    }
  }

  public void bar() {
    for (int i = 0; i < n; i++) {
      print("bar");
    }
  }
}
```
The same instance of FooBar will be passed to two different threads. Thread A will call foo() while thread B will call bar(). Modify the given program to output "foobar" n times.

 

Example 1:
```
Input: n = 1
Output: "foobar"
Explanation: There are two threads being fired asynchronously. One of them calls foo(), while the other calls bar(). "foobar" is being output 1 time.
```
Example 2:
```
Input: n = 2
Output: "foobarfoobar"
Explanation: "foobar" is being output 2 times.
```
{% raw %}
***          
{% endraw %}
这道题目的意思是多线程情况下如何保证代码执行的顺序，是一道经典的多线程的题目。

最容易想到的是用Java的synchronized加锁，然后flag变量控制执行顺序。

Java解法如下：
```java
class FooBar {
    volatile private int n;
    private int flag = 0;

    public FooBar(int n) {
        this.n = n;
    }

    public void foo(Runnable printFoo) throws InterruptedException {
        for (int i = 0; i < n; i++) {
            synchronized (this) {
                while (flag == 1) {
                    this.wait();
                }
                // printFoo.run() outputs "foo". Do not change or remove this line.
                printFoo.run();
                flag = 1;
                this.notifyAll();
            }
        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        for (int i = 0; i < n; i++) {
            synchronized (this) {
                while (flag == 0) {
                    this.wait();
                }
                // printBar.run() outputs "bar". Do not change or remove this line.
                printBar.run();
                flag = 0;
                this.notifyAll();
            }
        }
    }
}
```
当然我们可以使用更高级的工具，比如说 CyclicBarrier。         
注意：下面的代码有问题，不能确保并发的顺序！！
```java

class FooBar {
    private int n;
    private final CyclicBarrier barrier = new CyclicBarrier(2);

    public FooBar(int n) {
        this.n = n;
    }

    public void foo(Runnable printFoo) throws InterruptedException {

        for (int i = 0; i < n; i++) {
            // printFoo.run() outputs "foo". Do not change or remove this line.
            printFoo.run();
            try {
                barrier.await();
            } catch (BrokenBarrierException e) {
                e.printStackTrace();
            }
        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        for (int i = 0; i < n; i++) {
            try {
                barrier.await();
            } catch (BrokenBarrierException e) {
                e.printStackTrace();
            }
            // printBar.run() outputs "bar". Do not change or remove this line.
            printBar.run();
        }
    }
}
```

另一种正确的做法是使用信号量：Semaphore
```java
class FooBar {
    private int n;
    private final Semaphore foo = new Semaphore(0);
    private final Semaphore bar = new Semaphore(1);

    public FooBar(int n) {
        this.n = n;
    }

    public void foo(Runnable printFoo) throws InterruptedException {

        for (int i = 0; i < n; i++) {
            bar.acquire();
            // printFoo.run() outputs "foo". Do not change or remove this line.
            printFoo.run();
            foo.release();
        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        for (int i = 0; i < n; i++) {
            foo.acquire();
            // printBar.run() outputs "bar". Do not change or remove this line.
            printBar.run();
            bar.release();
        }
    }
}
```

