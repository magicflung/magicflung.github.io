---
title: 快速回顾MySQL：简单查询操作
copyright: true
tags: 数据库
categories: 数据库
description: MySQL第七篇：简单查询操作
abbrlink: 6ca57e6e
date: 2020-01-13 12:31:44
top:
---

> 前提要述：参考书籍《MySQL必知必会》
## 7.1 检索数据
为了查询出数据库表中的行（数据），使用SELECE语句。

格式：

```mysql
# 第一种
SELECT * FROM <table_name>;
# 第二种
SELECT field1,field2,... FROM <table_name>;
```

- 第一种写法使用\*通配符，会把表中行的列全部查询出来，而不必取一一列出全部列。但是不推荐使用，这跟INSERT语句的规范写法一样。**使用\*通配符，列的顺序一般是列在表定义中出现的顺序，但有时候并不是这样的，表的模式的变化（如添加或删除列）可能会导致顺序的变化。特别是像使用INSERT SELECT这样的语句，可能会报错，甚至可能会不会报错但是数据插入错误的列。**
- **推荐使用第二种，第二种查询方式可以查询表中行的全部列，也可以查询表中行的单列或多列。**
- 使用通配符注意事项：虽然使用通配符可能会省事，不用明确列出所需列，但检索不需要的列通常会降低检索和应用程序的性能。

例如：
- 单列查询：

```mysql
SELECT stu_name FROM student;
```

- 多列查询：

```mysql
SELECT stu_name, stu_sex FROM student;
```


- 查询全部列：


```mysql
SELECT * FROM student;
# 或（推荐）
SELECT stu_id, stu_name, stu_sex FROM student;
```

## 7.2 检索不同的行
因为在表中有时候需要进行去重操作，可以使用DISTINCT关键字，让SELECT查询的结果返回不同的值。

格式：

```
SELECT DISTINCT field1,field2,... FROM <stable_name>;
```

DISTINCT关键字的注意：
- 如果是单列，那么使用该关键字会将该列作为判断去重的条件。
- 如果是多列，那么会将该多列作为判断去重的条件。
- 使用这些单列或多列作为去重条件，如果在表中都是不相同的，那么就会将所有行都查出来。


## 7.3 限制结果
SELECT语句返回所有匹配的行。如果为了返回第一行或前几行，那么可以使用LIMIT子句。

格式：


```
# 第一种（n为整数）
SELECT field1,field2,... FROM <stable_name> LIMIT n;
# 第二种 (n，m为整数, n>=0, m>=1)
SELECT field1,field2,... FROM <stable_name> LIMIT n,m;
```
解释：
- 第一种写法，对于查询出的数据（行）只返回不多于n行。
- 第二种写法，LIMIT后的n表示查询的开始的位置，m表示要检索的行数。即，从第n行开始查询m行。
- LIMIT后n是从0开始的，所以检索出来的第一行为行0而不是行1。因此，LIMIT 1,1 是将检索出第二行而不是第一行。
- 如果LIMIT指定要检索的行数超过表中的行数，那么MySQL将返回它能返回的那么多行。

因为有人会把LIMIT n,m的语法搞混，比如LIMIT 3,4 是从行4开始的3行还是从行3开始的4行？如上面所述，它的意思是从行3开始的4行。  
所以，**MySQL 5支持LIMIT的另一种代替语法：LIMIT 4 OFFSET 3，意思是从行3开始取4行。**

## 7.4 使用完全限定的表名

到目前为止SQL例子只通过列名引用列。也可能会使用完全限定的名称来引用列（同时使用表明和列名），如下：

```
SELECT <stable_name>.field1,<stable_name>.field2,...
    FROM <stable_name>;
```
在单表时可能看不出什么差别，但是在多表查询时，**并且是那种有相关联的表，因为有可能两张表会有相同的列名，所以需要使用完全限定的表名来区分。**