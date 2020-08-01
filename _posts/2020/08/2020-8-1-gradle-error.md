---
layout: post
title: "Gradle构建报错解决：ERROR: Unable to find method 'org.gradle.api.artifacts.Configuration.isCanBeResolved()Z'."
categories: [行走的问题解决机]
description: ""
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}



晚上在构建Gradle项目时遇到了报错：

```txt
ERROR: Unable to find method 'org.gradle.api.artifacts.Configuration.isCanBeResolved()Z'.
Possible causes for this unexpected error include:
Gradle's dependency cache may be corrupt (this sometimes occurs after a network connection timeout.)
Re-download dependencies and sync project (requires network)

The state of a Gradle build process (daemon) may be corrupt. Stopping all Gradle daemons may solve this problem.
Stop Gradle build processes (requires restart)

Your project may be using a third-party plugin which is not compatible with the other plugins in the project or the version of Gradle requested by the project.

In the case of corrupt Gradle processes, you can also try closing the IDE and then killing all Java processes.
```

报错给了解决方法，但问题是我这个项目是普通的gradle项目，并不是Android项目。

解决方法是`File`->`Settings`->`Build`->`Build Tools`->`Gradle`

配置项目的`gradle`路径和`Gradle JVM`，先尝试使用IDEA自带的高版本gradle，如果项目中有指定gradle版本，选择项目指定的版本，如果还不行，可以尝试使用本地的gradle。

更换gradle版本重新构建项目后，就没有这个报错了。

