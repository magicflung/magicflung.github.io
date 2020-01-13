---
title: POJO类中布尔类型的变量千万别加is前缀！
copyright: true
tags: 阿里巴巴
categories: 阿里巴巴
description: POJO类中布尔类型的变量都不要加is前缀的讲解
abbrlink: 8fcf5eec
date: 2020-01-07 23:28:56
top:
---

## 前言
对应阿里巴巴开发手册第一章的命名风格的第八条。

**【强制】** POJO类中布尔类型的变量都不要加is前缀，否则部分框架解析会引起序列化错误。

```text
反例：定义为基本数据类型Boolean isDeleted; 的属性，它的方法名称也是 isDeleted()，
RPC框架在反向解析的时候，“误以为”对应的属性名称是deleted，导致属性获取不到抛出异常。
```

我对这个反例感觉有点怪怪的，基本数据类型Boolean？而且Boolean生成的getter方法是getXxx()，boolean生成的getter方法是isXxx()，疑惑，不知道是不是手册写错了还是我错了。

我认为这条很重要很重要。**前后端传递数据时，就有可能因为布尔类型变量的命名，导致前后端传送数据时传递失败。**因为这个布尔类型的数据解析不一致，在后端该命名是isXxx，而在前端发送表单是传送的数据是isXxx，但是响应时却是Xxx，说明传输到后端时会解析成Xxx（像spring会根据getter和setter来解析POJO类，而当我创建的是isXxx，那么自动生成的getter方法会是isXxx方法，然后解析时会去掉is，所以变成了Xxx），而后端原本是isXxx，导致找不到该属性，所以值也传递不了。

![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200107/20200107233149-309672.png)

![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200107/20200107233159-573377.png)

所以这是一个致命问题，但是可以避免啊。

## 详解
在Java中布尔类型有基本数据类型和包装类，所以有四种方式来定义一个布尔类型的变量：

```java
boolean isLive;
boolean live;
Boolean isLive;
Boolean live;
```

首先来总结上面的区别：
1. 四种中有两种是boolean，而另外两种是Boolean。可不要傻乎乎的说这两个类型是一样的，正确的是它们是有区别的。
2. 四种中有两种变量名是以is开头，另外两种没有。

---

先根据手册，可得布尔类型的变量命名不使用以is开头的。为什么？

来看看下面代码：

```java
// boolean isLive
class People {
    private boolean isLive;

    public boolean isLive() {
        return isLive;
    }

    public void setLive(boolean live) {
        isLive = live;
    }
}

// boolean live
class People {
    private boolean live;

    public boolean isLive() {
        return live;
    }

    public void setLive(boolean live) {
        this.live = live;
    }
}

// Boolean isLive
class People {
    private Boolean isLive;

    public Boolean getLive() {
        return isLive;
    }

    public void setLive(Boolean live) {
        isLive = live;
    }
}

// Boolean live
class People {
    private Boolean live;

    public Boolean getLive() {
        return live;
    }

    public void setLive(Boolean live) {
        this.live = live;
    }
}
```
噫，通过代码和idea自动生成的getter和setter方法，可得到：
1. boolean类型的getter方法是isXxx()的形式。
2. Boolean类型的getter方法是getXxx()的形式。
3. 这两个类型的setter方法相同。

其实isXxx()方法和getXxx()都是一样的，都是会解析成Xxx，但是只有布尔类型才有isXxx()方法，所以两种可以互换。比如把Boolean类型的getXxx改成isXxx，或者把boolean类型的isXxx改成getXxx。

一般情况下，其实布尔类型使用isXxx或不使用没有区别，但是在序列化时才可以看到区别，特别是现在开发中，都是序列化或者自动序列化的，所以可能因为命名的问题导致错误。

现在就先解决使用isXxx和使用Xxx的问题，我们拿常用的JSON序列化举例，比如看看fastJson、jackson和Gson之间有何区别：（还好参考了别人的博文不然我自己举的例子只是fastJson，有参考地址）


```java
// boolean isLive
class People1 implements Serializable {
    private boolean isLive;

    public boolean isLive() {
        return isLive;
    }

    public void setLive(boolean live) {
        isLive = live;
    }
}
// boolean live
class People2 implements Serializable {
    private boolean live;

    public boolean isLive() {
        return live;
    }

    public void setLive(boolean live) {
        this.live = live;
    }
}
public class PeopleTest {
    public static void main(String[] args) throws JsonProcessingException {
        ObjectMapper jackson = new ObjectMapper();
        Gson gson = new Gson();

        People1 people1 = new People1();
        people1.setLive(true);
        System.out.println("boolean isLive");
        System.out.println("fastJson：" + JSON.toJSONString(people1));

        System.out.println("jackson：" + jackson.writeValueAsString(people1));

        System.out.println("Gson：" + gson.toJson(people1));

        People2 people2 = new People2();
        people2.setLive(true);
        System.out.println("boolean live");
        System.out.println("fastJson：" + JSON.toJSONString(people2));
        System.out.println("jackson：" + jackson.writeValueAsString(people2));
        System.out.println("Gson：" + gson.toJson(people2));
    }
}
```


去maven仓库访问速度有时太慢了，所以下面提供了fastJson、jackson和Gson的仓库链接，自己拿去耍耍。

```xml
        <!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.58</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.9.8</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.5</version>
        </dependency>
```

运行结果：

```
boolean isLive
fastJson：{"live":true}
jackson：{"live":true}
Gson：{"isLive":true}
boolean live
fastJson：{"live":true}
jackson：{"live":true}
Gson：{"live":true}
```

根据这些结果就可以得出：
1. 使用布尔类型的变量命名为isXxx，fastJson和jackson在把对象序列化成json字符串时，是通过反射遍历出该类中的**所有getter方法(或isXxx方法)**，得到isLive()方法，然后根据JavaBean规范，它会认为这是live属性，然后序列化成json。
2. 而Gson并不是这么做的，他是通过反射遍历该类中的**所有属性**，并把其值序列化成json。

可得由于不同的序列化工具，在进行序列化的时候使用到的策略是不一样的，所以，对于同一个类的同一个对象的序列化结果可能是不同的。 

现在，不同的序列化框架得到的json内容并不相同，如果对于同一个对象，我使用fastjson进行序列化，再使用Gson反序列化会发生什么？


```java
// boolean isLive
class People1 implements Serializable {
    private boolean isLive;

    public boolean isLive() {
        return isLive;
    }

    public void setLive(boolean live) {
        isLive = live;
    }

    @Override
    public String toString() {
        return "People1{" +
                "isLive=" + isLive +
                '}';
    }
}
// boolean live
class People2 implements Serializable {
    private boolean live;

    public boolean isLive() {
        return live;
    }

    public void setLive(boolean live) {
        this.live = live;
    }

    @Override
    public String toString() {
        return "People2{" +
                "live=" + live +
                '}';
    }
}
public class PeopleTest {
    public static void main(String[] args) throws JsonProcessingException {
        Gson gson = new Gson();

        People1 people1 = new People1();
        people1.setLive(true);


        People2 people2 = new People2();
        people2.setLive(true);


        System.out.println("boolean isLive::" + gson.fromJson(JSON.toJSONString(people1), People1.class));
        System.out.println("boolean live::" + gson.fromJson(JSON.toJSONString(people2), People2.class));

    }
}
```
结果：

![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200110/20200110110246-778766.png)

会发现，在People1类中已经把isLive设置成true，现在怎么变成false？

原因是因为JSON框架通过扫描所有的getter后发现有一个isLive方法，然后根据JavaBeans的规范，解析出变量名为live，把model对象序列化城字符串后内容为{“live”: true}。  
然后根据{“live”: true}这个json串，Gson框架在通过解析后，通过反射寻找Model类中的live属性，但是Model类中只有isLive属性，找不到对应的属性，所以，最终反序列化后的Model类的对象中，isLive则会使用默认值false。

所以这样会导致前台的布尔类型的数据传送不到后端，因为前端传过来的会把命名为isXxx的变量解析成Xxx，而我们POJO类中的布尔类型的属性是isXXX，导致在POJO类中找不到Xxx，所以就导致传递值失败。我们必须避免这样的致命问题。

最终，解释了：*POJO类中布尔类型的变量都不要加is前缀*。

---

下面再来解释，是使用Boolean类型还是使用boolean。这里可以看开发手册 *1.4 OOP规约的第11条*。

> 所有的POJO类属性必须使用包装数据类型。  
> RPC方法的返回值和参数必须使用包装数据类型。  
> 所有的局部变量使用基本数据类型。

为什么？

很简单，看这两种类型的区别，boolean类型的默认值为false；而Boolean类型的默认值是null。

举个栗子：**在学校考试的时候，我们要录入学生成绩，假设使用Integer类型来录入，那么学生要是没有来考试，那么该成绩就是null，学生考0分，说明学生有来考试，但是考了0分；使用int类型来录入，注意int类型默认值为0，那么成绩如果是0，我们不能确定该学生是没有来考试的呢？还是有来考试但考了0分。**



参考：
[为什么阿里巴巴开发手册强制规定POJO 类中布尔类型的变量，都不要加 is 前缀](https://blog.csdn.net/u011732080/article/details/102853470)