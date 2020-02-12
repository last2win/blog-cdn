---
layout: post
title: "LeetCode 1242. Web Crawler Multithreaded--Java 解法--网路爬虫并发系列--ConcurrentHashMap/Collections.synchronizedList"
categories: [LeetCode]
description: "LeetCode 1242. Web Crawler Multithreaded--Java 解法--网路爬虫并发系列--ConcurrentHashMap/Collections.synchronizedList"
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


题目地址：[Web Crawler Multithreaded - LeetCode](https://leetcode.com/problems/web-crawler-multithreaded/)
{% raw %}
***          
{% endraw %}


Given a url startUrl and an interface HtmlParser, implement a Multi-threaded web crawler to crawl all links that are under the same hostname as startUrl. 

Return all urls obtained by your web crawler in any order.

Your crawler should:

- Start from the page: startUrl     
- Call HtmlParser.getUrls(url) to get all urls from a webpage of given url.       
- Do not crawl the same link twice.      
- Explore only the links that are under the same hostname as startUrl.       


As shown in the example url above, the hostname is example.org. For simplicity sake, you may assume all urls use http protocol without any port specified. For example, the urls http://leetcode.com/problems and http://leetcode.com/contest are under the same hostname, while urls http://example.org/test and http://example.com/abc are not under the same hostname.

The HtmlParser interface is defined as such: 
```java
interface HtmlParser {
  // Return a list of all urls from a webpage of given url.
  // This is a blocking call, that means it will do HTTP request and return when this request is finished.
  public List<String> getUrls(String url);
}
```
Note that getUrls(String url) simulates performing a HTTP request. You can treat it as a blocking function call which waits for a HTTP request to finish. It is guaranteed that getUrls(String url) will return the urls within 15ms.  Single-threaded solutions will exceed the time limit so, can your multi-threaded web crawler do better?

Below are two examples explaining the functionality of the problem, for custom testing purposes you'll have three variables urls, edges and startUrl. Notice that you will only have access to startUrl in your code, while urls and edges are not directly accessible to you in code.

 

Follow up:

- Assume we have 10,000 nodes and 1 billion URLs to crawl. We will deploy the same software onto each node. The software can know about all the nodes. We have to minimize communication between machines and make sure each node does equal amount of work. How would your web crawler design change?    
- What if one node fails or does not work?        
- How do you know when the crawler is done?         
 

Example 1:


```
Input:
urls = [
  "http://news.yahoo.com",
  "http://news.yahoo.com/news",
  "http://news.yahoo.com/news/topics/",
  "http://news.google.com",
  "http://news.yahoo.com/us"
]
edges = [[2,0],[2,1],[3,2],[3,1],[0,4]]
startUrl = "http://news.yahoo.com/news/topics/"
Output: [
  "http://news.yahoo.com",
  "http://news.yahoo.com/news",
  "http://news.yahoo.com/news/topics/",
  "http://news.yahoo.com/us"
]
```
Example 2:


```
Input: 
urls = [
  "http://news.yahoo.com",
  "http://news.yahoo.com/news",
  "http://news.yahoo.com/news/topics/",
  "http://news.google.com"
]
edges = [[0,2],[2,1],[3,2],[3,1],[3,0]]
startUrl = "http://news.google.com"
Output: ["http://news.google.com"]
Explanation: The startUrl links to all other pages that do not share the same hostname.
 
```
Constraints:

- 1 <= urls.length <= 1000        
- 1 <= urls[i].length <= 300        
- startUrl is one of the urls.        
- Hostname label must be from 1 to 63 characters long, including the dots, may contain only the ASCII letters from 'a' to 'z', digits from '0' to '9' and the hyphen-minus character ('-').        
- The hostname may not start or end with the hyphen-minus character ('-').         
- See:  https://en.wikipedia.org/wiki/Hostname#Restrictions_on_valid_hostnames        
- You may assume there're no duplicates in url library.        


{% raw %}
***          
{% endraw %}


这道题目很新颖，意思是写一个模拟爬虫程序，从一个开始页面，爬取属于这个域名的所有网页，LeetCode提供了爬虫的接口。

题目要求必须使用多线程爬虫，不然会超时。

1.用set来存储已经爬取过的网页,这个set需要支持多线程并发修改，我使用`ConcurrentHashMap`

2.存储结果的List也只要支持多线程并发，我使用`Collections.synchronizedList`



Java解法如下：
```java
class Solution {
    private final Set<String> set = Collections.newSetFromMap(new ConcurrentHashMap<String, Boolean>());
    private final List<String> result = Collections.synchronizedList(new ArrayList<String>());
    private String HOSTNAME = null;

    public boolean judgeHostname(String url) {
        int idx = url.indexOf('/', 7);
        String hostName = (idx != -1) ? url.substring(0, idx) : url;
        return hostName.equals(HOSTNAME);
    }

    private void initHostName(String url) {
        int idx = url.indexOf('/', 7);
        HOSTNAME = (idx != -1) ? url.substring(0, idx) : url;
    }

    public void getUrl(String startUrl, HtmlParser htmlParser) {
        result.add(startUrl);
        List<String> res = htmlParser.getUrls(startUrl);
        List<Thread> threads = new ArrayList<>();
        for (String url : res) {
            if (judgeHostname(url) && !set.contains(url)) {
                set.add(url);
                threads.add(new Thread(() -> {
                    getUrl(url, htmlParser);
                }));
            }
        }
        for (Thread thread : threads) {
            thread.start();
        }
        try {
            for (Thread thread : threads) {
                thread.join();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public List<String> crawl(String startUrl, HtmlParser htmlParser) {
        initHostName(startUrl);
        set.add(startUrl);
        getUrl(startUrl, htmlParser);
        return result;
    }
}
```
这个解法的缺点是可能会在运行过程中产生大量的无用线程，可以使用线程池进行并发操作，减少创建线程的开销。

使用线程池的最大难点在于如何确定所有任务都执行完成，可以关闭线程池。

我尝试使用 Future, CountDownLatch，但效果都不好。最关键的是运行速度没有直接新建线程快！！