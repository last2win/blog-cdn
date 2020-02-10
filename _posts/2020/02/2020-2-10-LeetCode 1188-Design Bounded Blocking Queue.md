---
layout: post
title: "LeetCode 1188. Design Bounded Blocking Queue--并发系列--Java 解法--设计有界阻塞队列--使用ArrayBlockingQueue和LinkedList"
categories: [LeetCode]
description: "LeetCode 1188. Design Bounded Blocking Queue--并发系列--Java 解法--设计有界阻塞队列--使用ArrayBlockingQueue和LinkedList"
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
题目地址：[Design Bounded Blocking Queue - LeetCode](https://leetcode.com/problems/design-bounded-blocking-queue/)
{% raw %}
***          
{% endraw %}
Implement a thread safe bounded blocking queue that has the following methods:

- BoundedBlockingQueue(int capacity) The constructor initializes the queue with a maximum capacity.      
- void enqueue(int element) Adds an element to the front of the queue. If the queue is full, the calling thread is blocked until the queue is no longer full.            
- int dequeue() Returns the element at the rear of the queue and removes it. If the queue is empty, the calling thread is blocked until the queue is no longer empty.      
- int size() Returns the number of elements currently in the queue.         

Your implementation will be tested using multiple threads at the same time. Each thread will either be a producer thread that only makes calls to the enqueue method or a consumer thread that only makes calls to the dequeue method. The size method will be called after every test case.

Please do not use built-in implementations of bounded blocking queue as this will not be accepted in an interview.

 

Example 1:
```
Input:
1
1
["BoundedBlockingQueue","enqueue","dequeue","dequeue","enqueue","enqueue","enqueue","enqueue","dequeue"]
[[2],[1],[],[],[0],[2],[3],[4],[]]

Output:
[1,0,2,2]

Explanation:
Number of producer threads = 1
Number of consumer threads = 1

BoundedBlockingQueue queue = new BoundedBlockingQueue(2);   // initialize the queue with capacity = 2.

queue.enqueue(1);   // The producer thread enqueues 1 to the queue.
queue.dequeue();    // The consumer thread calls dequeue and returns 1 from the queue.
queue.dequeue();    // Since the queue is empty, the consumer thread is blocked.
queue.enqueue(0);   // The producer thread enqueues 0 to the queue. The consumer thread is unblocked and returns 0 from the queue.
queue.enqueue(2);   // The producer thread enqueues 2 to the queue.
queue.enqueue(3);   // The producer thread enqueues 3 to the queue.
queue.enqueue(4);   // The producer thread is blocked because the queue's capacity (2) is reached.
queue.dequeue();    // The consumer thread returns 2 from the queue. The producer thread is unblocked and enqueues 4 to the queue.
queue.size();       // 2 elements remaining in the queue. size() is always called at the end of each test case.
 
```
Example 2:
```
Input:
3
4
["BoundedBlockingQueue","enqueue","enqueue","enqueue","dequeue","dequeue","dequeue","enqueue"]
[[3],[1],[0],[2],[],[],[],[3]]

Output:
[1,0,2,1]

Explanation:
Number of producer threads = 3
Number of consumer threads = 4

BoundedBlockingQueue queue = new BoundedBlockingQueue(3);   // initialize the queue with capacity = 3.

queue.enqueue(1);   // Producer thread P1 enqueues 1 to the queue.
queue.enqueue(0);   // Producer thread P2 enqueues 0 to the queue.
queue.enqueue(2);   // Producer thread P3 enqueues 2 to the queue.
queue.dequeue();    // Consumer thread C1 calls dequeue.
queue.dequeue();    // Consumer thread C2 calls dequeue.
queue.dequeue();    // Consumer thread C3 calls dequeue.
queue.enqueue(3);   // One of the producer threads enqueues 3 to the queue.
queue.size();       // 1 element remaining in the queue.

Since the number of threads for producer/consumer is greater than 1, we do not know how the threads will be scheduled in the operating system, even though the input seems to imply the ordering. Therefore, any of the output [1,0,2] or [1,2,0] or [0,1,2] or [0,2,1] or [2,0,1] or [2,1,0] will be accepted.
```

{% raw %}
***          
{% endraw %}

这道题目是LeetCode并发系列题目，题目的意思很简单，设计一个有界阻塞队列，难点在于要求是线程安全的。

Java标准库中线程安全的队列(queue)有几个，其中`ArrayBlockingQueue`适合这道题目。


Java解法如下：
```java
class BoundedBlockingQueue {
    private ArrayBlockingQueue queue = null;

    public BoundedBlockingQueue(int capacity) {
        queue = new ArrayBlockingQueue(capacity);
    }

    public void enqueue(int element) throws InterruptedException {
        queue.put(element);
    }

    public int dequeue() throws InterruptedException {
        return (int) queue.take();
    }

    public int size() {
        return queue.size();
    }
}
```
但如果面试的时候出现这道题目，肯定不能这么做

最容易想到的方法是使用锁，确保同时只有一个线程访问数据，可以使用`synchronized`关键字。

解法如下：
```java
class BoundedBlockingQueue {
    private final Queue queue;
    private final Object mutex = new Object();
    private int capacity;

    public BoundedBlockingQueue(int capacity) {
        queue = new LinkedList<Integer>();
        this.capacity = capacity;
    }

    public void enqueue(int element) throws InterruptedException {
        synchronized (mutex) {
            while (size() >= capacity) {
                mutex.wait();
            }
            queue.add(element);
            mutex.notifyAll();
        }
    }

    public int dequeue() throws InterruptedException {
        int result;
        synchronized (mutex) {
            while (size() <= 0) {
                mutex.wait();
            }
            result = (int) queue.remove();
            mutex.notifyAll();
        }
        return result;
    }

    public int size() {
        return queue.size();
    }
}
```
