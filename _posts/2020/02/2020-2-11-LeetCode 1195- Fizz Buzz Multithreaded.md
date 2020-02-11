---
layout: post
title: "LeetCode 1195. Fizz Buzz Multithreaded--并发系列题目--Java 解法--AtomicInteger/CountDownLatch/CyclicBarrier"
categories: [LeetCode]
description: "LeetCode 1195. Fizz Buzz Multithreaded--并发系列题目--Java 解法--AtomicInteger/CountDownLatch/CyclicBarrier"
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


题目地址：[Fizz Buzz Multithreaded - LeetCode](https://leetcode.com/problems/fizz-buzz-multithreaded/)

{% raw %}
***          
{% endraw %}


Write a program that outputs the string representation of numbers from 1 to n, however:

If the number is divisible by 3, output "fizz".
If the number is divisible by 5, output "buzz".
If the number is divisible by both 3 and 5, output "fizzbuzz".
For example, for n = 15, we output: 1, 2, fizz, 4, buzz, fizz, 7, 8, fizz, buzz, 11, fizz, 13, 14, fizzbuzz.

Suppose you are given the following code:

```
class FizzBuzz {
  public FizzBuzz(int n) { ... }               // constructor
  public void fizz(printFizz) { ... }          // only output "fizz"
  public void buzz(printBuzz) { ... }          // only output "buzz"
  public void fizzbuzz(printFizzBuzz) { ... }  // only output "fizzbuzz"
  public void number(printNumber) { ... }      // only output the numbers
}
```
Implement a multithreaded version of FizzBuzz with four threads. The same instance of FizzBuzz will be passed to four different threads:

Thread A will call fizz() to check for divisibility of 3 and outputs fizz.
Thread B will call buzz() to check for divisibility of 5 and outputs buzz.
Thread C will call fizzbuzz() to check for divisibility of 3 and 5 and outputs fizzbuzz.
Thread D will call number() which should only output the numbers.



{% raw %}
***          
{% endraw %}


这道题目是LeetCode的并发系列题目，题目意思也很简单，4个线程分别做不同的事情，难的点是需要4个线程都完成任务后都进行等待。

总结一下任务过程：

1.调用4个函数，然后等待

2.4个函数调用都完成后，n+1.

3.如果一个函数重复调用，可以选择阻塞线程，或者直接跳过。

既然提到多线程的任务同步，很容易想到Java线程库中的`CountDownLatch`和`CyclicBarrier`。

先介绍一下：

> CountDownLatch 主要用来解决一个线程等待多个线程的场景，可以类比旅游团团长要等待所有的游客到齐才能去下一个景点；而 CyclicBarrier 是一组线程之间互相等待，更像是几个驴友之间不离不弃。除此之外 CountDownLatch 的计数器是不能循环利用的，也就是说一旦计数器减到 0，再有线程调用 await()，该线程会直接通过。但 CyclicBarrier 的计数器是可以循环利用的，而且具备自动重置的功能，一旦计数器减到 0 会自动重置到你设置的初始值。除此之外，CyclicBarrier 还可以设置回调函数，可以说是功能丰富。

但这道题目的难点在于4个线程要相互等待，而且一个函数可能被重复调用，等4个函数都执行完成后才能对n进行操作。

在思考后我觉得还是一个函数直接完成循环知道完成任务更容易实现。

Java解法如下：
```java

class FizzBuzz {

    private int n;
    private int currentNumber = 1;
    private final Object mutex = new Object();

    public FizzBuzz(int n) {
        this.n = n;
    }

    // printFizz.run() outputs "fizz".
    public void fizz(Runnable printFizz) throws InterruptedException {
        synchronized (mutex) {
            while (currentNumber <= n) {
                if (currentNumber % 3 != 0 || currentNumber % 5 == 0) {
                    mutex.wait();
                    continue;
                }
                printFizz.run();
                currentNumber += 1;
                mutex.notifyAll();
            }
        }
    }

    // printBuzz.run() outputs "buzz".
    public void buzz(Runnable printBuzz) throws InterruptedException {
        synchronized (mutex) {
            while (currentNumber <= n) {
                if (currentNumber % 5 != 0 || currentNumber % 3 == 0) {
                    mutex.wait();
                    continue;
                }
                printBuzz.run();
                currentNumber += 1;
                mutex.notifyAll();
            }
        }
    }

    // printFizzBuzz.run() outputs "fizzbuzz".
    public void fizzbuzz(Runnable printFizzBuzz) throws InterruptedException {
        synchronized (mutex) {
            while (currentNumber <= n) {
                if (currentNumber % 15 != 0) {
                    mutex.wait();
                    continue;
                }
                printFizzBuzz.run();
                currentNumber += 1;
                mutex.notifyAll();
            }
        }
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void number(IntConsumer printNumber) throws InterruptedException {
        synchronized (mutex) {
            while (currentNumber <= n) {
                if (currentNumber % 3 == 0 || currentNumber % 5 == 0) {
                    mutex.wait();
                    continue;
                }
                printNumber.accept(currentNumber);
                currentNumber += 1;
                mutex.notifyAll();
            }
        }
    }
}
```
这里用了锁解决并发问题，但如果想要不用锁，循环轮询等待的话，可以使用`AtomicInteger`。