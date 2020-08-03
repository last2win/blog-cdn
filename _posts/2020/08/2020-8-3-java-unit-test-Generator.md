---
layout: post
title: "IDEA-Java自动生成单元测试"
categories: [Java]
description: "Java IDEA unit test Generator-Squaretest 自动生成测试函数"
---

此文首发于我的Jekyll博客：[zhang0peter的个人博客](https://zhang0peter.com)         

{% raw %}
***          
{% endraw %}



最近在写单元测试，感觉写Mock写烦了，于是想看看有没有现成的spring项目的单元测试生成工具。

网上找到了一个Java单元测试回答的集合：[Automatic generation of unit tests for Java? - Stack Overflow](https://stackoverflow.com/questions/2131935/automatic-generation-of-unit-tests-for-java)

# Squaretest


官网：[Squaretest - Java Unit Test Generator for IntelliJ IDEA](https://squaretest.com/)

安装方法是从IDEA插件仓库中安装`Squaretest`

使用方法：[SquaretestLLC/Squaretest:  the Squaretest plugin for IntelliJ IDEA](https://github.com/SquaretestLLC/Squaretest)

在使用了一次后，感觉真的好用，尤其是在编写test文件时自动生成测试函数，非常好用。

spring service示例代码：
```java
import java.util.List;
import lombok.Data;
import org.apache.commons.lang3.StringUtils;
import org.springframework.stereotype.Service;

@Service
public class springServiceImpl {

    @Data
    static public class Person {
        private String name;
        private Integer age;
    }

    public boolean judge(List<Person> persons) {
        for (Person person : persons
        ) {
            if (StringUtils.isBlank(person.getName()) || person.getAge() == null) {
                return false;
            }
        }
        return true;
    }
}
```
然后在测试类中打`public`就会有自动补全函数如下：
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = DemoApplication.class)
@ActiveProfiles("test")
public class springServiceImplTest {

    @Autowired
    private springServiceImpl springService;

    @Test
    public void testJudge() {
        // Setup
        final springServiceImpl.Person person = new springServiceImpl.Person();
        person.setName("name");
        person.setAge(0);
        final List<springServiceImpl.Person> persons = List.of(person);

        // Run the test
        final boolean result = springService.judge(persons);

        // Verify the results
        assertTrue(result);
    }
}
```

真心好用，尤其是自动配置了set和get，不需要手动配，省了很多时间。


# 其他

Java的unit test Generator还有很多，但大部分都很久不更新了：

[Randoop: Automatic unit test generation for Java](https://randoop.github.io/randoop/)：最后更新时间：2020-07-28，是少数还在更新的单元测试生成器，但使用起来不方便，我就没试了。

[JUnitGenerator V2.0 - plugin for IntelliJ IDEA and Android Studio | JetBrains](https://plugins.jetbrains.com/plugin/3064-junitgenerator-v2-0): 最后更新时间：2015-05-06   

CodePro Analytix/CodePlex AnalytiX 自从2011年被谷歌收购后就没有更新了。

[JUnit-Tools](http://junit-tools.org/index.php)：只支持Eclipse，最后更新时间：2018-11-09

[EvoSuite | Automatic Test Suite Generation for Java](https://www.evosuite.org/)：最后更新时间：2018-04-06

