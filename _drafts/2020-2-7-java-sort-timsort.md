---
layout: post
title: "Java内置排序算法：Timsort详解"
categories: [Java]
description: ""
keywords: Java
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}

今天在看Java的内置排序算法后：[Java数组排序： Array-ArrayList-List-Collections.sort()/List.sort()/Arrays.sort()](https://zhang0peter.com/2020/02/07/java-array-list-sort/)注意到了排序代码的具体实现：

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
            legacyMergeSort(a); //旧版的 MergeSort 已被弃用
        else
            ComparableTimSort.sort(a, 0, a.length, null, 0, 0);
    }
```
关于用`TimSort`而不用快排`Dual-Pivot Quicksort`，网上有个讨论：[algorithm - Comparison between timsort and quicksort - Stack Overflow](https://stackoverflow.com/questions/7770230/comparison-between-timsort-and-quicksort)

快排适合原始数组是因为内存的局部性和缓存。

里面有个推测说，对象数组存放的仅仅是对象的引用，而实际的比较需要从堆中读取对象，因此`TimSort`更合适。

Python中的所有数据都是对象，因此Python的内置排序算法也是`TimSort`：[sorting - What algorithm does python's sorted() use? - Stack Overflow](https://stackoverflow.com/questions/10948920/what-algorithm-does-pythons-sorted-use)

下面详细介绍一下`TimSort`的实现过程。

`TimSort`的本质是插入排序和归并排序的结合体，有以下特点：

1.是稳定的排序算法，最坏时间复杂度为O(N*log(N))

2.对小块进行插入排序，然后进行归并排序

全部的代码其实没几行：

```java
    static <T> void sort(T[] a, int lo, int hi, Comparator<? super T> c,
                         T[] work, int workBase, int workLen) {
        assert c != null && a != null && lo >= 0 && lo <= hi && hi <= a.length;

        int nRemaining  = hi - lo;
        if (nRemaining < 2)
            return;  // Arrays of size 0 and 1 are always sorted

        // If array is small, do a "mini-TimSort" with no merges
        //private static final int MIN_MERGE = 32
        //如果数组长度小于32，调用 binarySort，也就是二分插入排序
        if (nRemaining < MIN_MERGE) {
            int initRunLen = countRunAndMakeAscending(a, lo, hi, c);
            binarySort(a, lo, hi, lo + initRunLen, c);
            return;
        }

        /**
         * March over the array once, left to right, finding natural runs,
         * extending short natural runs to minRun elements, and merging runs
         * to maintain stack invariant.
         */
        TimSort<T> ts = new TimSort<>(a, c, work, workBase, workLen);
        int minRun = minRunLength(nRemaining);
        do {
            // Identify next run
            int runLen = countRunAndMakeAscending(a, lo, hi, c);

            // If run is short, extend to min(minRun, nRemaining)
            if (runLen < minRun) {
                int force = nRemaining <= minRun ? nRemaining : minRun;
                binarySort(a, lo, lo + force, lo + runLen, c);
                runLen = force;
            }

            // Push run onto pending-run stack, and maybe merge
            ts.pushRun(lo, runLen);
            ts.mergeCollapse();

            // Advance to find next run
            lo += runLen;
            nRemaining -= runLen;
        } while (nRemaining != 0);

        // Merge all remaining runs to complete sort
        assert lo == hi;
        ts.mergeForceCollapse();
        assert ts.stackSize == 1;
    }
```



