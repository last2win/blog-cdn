---
layout: post
title: "Java跨类使用ThreadLocal：线程中的全局变量"
categories: [java]
description: "使用ThreadLocal进行跨类传递线程全局变量"
---

此文首发于我的个人博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

最近由于新增了一个业务场景，导致很多函数的逻辑都需要改变，需要先判断是否属于该业务场景，再做对应的逻辑，原本的打算是在入口函数新增一个变量，然后在每次需要进行逻辑判断时都把flag变量传进去。

但这样做既不优雅，后续如果还要增加业务场景，头都要改大了，如果有一个方法可以传递全局变量，而且仅限于当前线程就好了。于是就想到了ThreadLocal这个之前很少用到的技巧。

以微服务架构为例，dubbo服务提供方在收到调用方的请求后，会把这个请求分配给一个线程进行处理。一般来说，一个请求会一直由同一个线程处理，中间不会切换线程，所以如果有一个线程中共享的变量，可以当全局变量使用。

ThreadLocal实现的就是一个线程中的全局变量，与真正的全局变量的区别在于ThreadLocal的变量是每个线程中的全局变量，也就是说不同线程访问到的值是不一样的。

ThreadLocal最朴素的实现是`Map<ThreadId, variable>`，每个线程id对应唯一的一个变量。但Java源码并不是Map<ThreadId, variable>的实现。这是因为如果多个线程访问同一个map，这个map需要是线程安全的，构造比较麻烦。Java采用了更简单粗暴的做法：每个线程都有自己的ThreadLocal专属map，里面可以存放多个ThreadLocal变量，这样就解决了多线程同时操作一个map带来的多线程并发问题。

因为要把ThreadLocal的变量当做全局变量使用，需要把变量与初始化函数写在通用的类中，如DDD领域模型中写在Common模块。

具体的实现如下：
```java
public class ThreadLocalContextHolder {
   /**
    * 不同业务设置不同的业务场景，如：业务A设置值为1，业务B设置值为2...
    */
   private static ThreadLocal<Integer> sceneThreadLocal = new ThreadLocal<>();


   public static Integer getScene() {
       return sceneThreadLocal.get();
   }

   public static void initScene(Integer scene) {
       if (ThreadLocalContextHolder.sceneThreadLocal == null) {
           ThreadLocalContextHolder.sceneThreadLocal = new ThreadLocal<>();
       }
       ThreadLocalContextHolder.sceneThreadLocal.set(scene);
   }

   public static void clearScene() {
       initScene(null);
   }

}
```

说一下我的用法，比如说有业务场景A和B，会调用不同的dubbo接口，我会在dubbo入口处设置好flag变量，接着调用统一的逻辑处理函数。然后在需要对不同业务进行不同处理的场景中，获取ThreadLocal中的变量，进行逻辑判断，这样就减少了变量的传递，代码也就更整洁了。




