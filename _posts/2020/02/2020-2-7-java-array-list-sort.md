---
layout: post
title: "Java数组排序： Array-ArrayList-List-Collections.sort()/List.sort()/Arrays.sort()"
categories: [Java]
description: ""
keywords: Java
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}


之前写了一篇博客：[Java：获取数组中的子数组的多种方法](https://blog.csdn.net/zhangpeterx/article/details/88716563)

现在在做LeetCode题目时想要对数组进行排序，于是想到Java中是否存在C++的标准库中的std::sort()。

Java的排序分为2种，一种是ArrayList或者List；另一种则是Array

## ArrayList/List 的排序：Collections.sort()/List.sort()

先说一下排序的各种方法的具体使用示例：sort List

代码如下：
```java
        List<String> strings = Arrays.asList("zoo", "foo", "bar", "baz");
        Collections.sort(strings); 
        System.out.println(strings.toString());
```
```java
        List<String> strings = Arrays.asList("zoo", "foo", "bar", "baz");
        strings.sort(null);
        System.out.println(strings.toString());
```

`Collections.sort()`函数本质上是调用`List.sort()`:

```java
    //Collections.sort()
    public static <T extends Comparable<? super T>> void sort(List<T> list) {
        list.sort(null);
    }
```
对于所有实现了接口`List`的类，如`ArrayList`，自然继承了函数`sort`，而`List.sort()`本质上调用了`Arrays.sort()`。


```java
    //List.sort()
    default void sort(Comparator<? super E> c) {
        Object[] a = this.toArray();
        Arrays.sort(a, (Comparator) c);
        ListIterator<E> i = this.listIterator();
        for (Object e : a) {
            i.next();
            i.set((E) e);
        }
    }
```
而`Arrays.sort()`函数的具体实现如下：
```java
    public static <T> void sort(T[] a, Comparator<? super T> c) {
        if (c == null) {
            sort(a);
        } else {
            if (LegacyMergeSort.userRequested)
                legacyMergeSort(a, c);
            else
                TimSort.sort(a, 0, a.length, c, null, 0, 0);
        }
    }
    public static void sort(Object[] a) {
        if (LegacyMergeSort.userRequested)
            legacyMergeSort(a); //旧的排序算法，不推荐
        else
            ComparableTimSort.sort(a, 0, a.length, null, 0, 0);
    }
    private static void legacyMergeSort(Object[] a) {
        Object[] aux = a.clone();
        mergeSort(aux, a, 0, a.length, 0);
    }
```

至于`TimSort`这种排序算法的具体实现，参考我的这篇博客:[]()

如果想自定义`Comparator`，参考这篇博客：[Java 8 Comparator: How to Sort a List - DZone Java](https://dzone.com/articles/java-8-comparator-how-to-sort-a-list)

## Array 的排序：Arrays.sort()

List中存放的只能是类的对象，或者包装类的对象，如：Long, Integer。

而如果我们把对象直接存放在数组中，也就是 Array 中，如 存放int, long在 Array 数组中，排序的方法如下：

```java
        int[] nums = {-2, 5, -1};
        Arrays.sort(nums);
        System.out.println(Arrays.toString(nums));
```
可以看出不管是基本类型数组，还是List，最终都是调用`Arrays.sort`进行最终的排序。