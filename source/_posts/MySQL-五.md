---
title: M快速回顾MySQL：插入操作
copyright: true
tags: 数据库
categories: 数据库
description: MySQL第五篇：插入操作
abbrlink: d51ba115
date: 2020-01-13 12:23:09
top:
---

> 前提要述：参考书籍《MySQL必知必会》  

《MySQL必知必会》是先讲了查询，但是没有记录就无法查询，所以先将如何添加数据。

表已经知道怎么创建了，随便创两张。


## 5.1 插入数据
MySQL使用 INSERT来插入（或添加）行（记录）到数据库表中。插入可用以下几种方式使用：
- 插入完整的行（记录）；
- 插入行的一部分；
- 插入多行；
- 插入某些查询结果。

## 5.2 插入完整的行（记录）

什么叫完整的行，即插入的有效数据都可以对应表中的每一列。

把数据插入表中，最简单的方法是使用基本的INSERT语法，需要指定表名和被插入到新行中的值，格式：

```mysql
# 写法：
INSERT INTO <table_name> VALUES(value1，value2，...);
# 规范写法：
INSERT INTO <table_name>(field1, field2, ...) VALUES(value1，value2，...);
```
 解释：

- 如果使用第一种写法，**虽然很简单，但是并不安全，应该尽量避免使用。因为该语句高度依赖于表中列的次序，并且还依赖其次序容易获得的信息。即使可得到这种次序信息，也不能保证下次表结构变动后各个列保持完全相同的次序**。所以在开发中如果这样写，那么就是把插入语句写死了。因为数据库表中的列的顺序有可能可能会改变，一旦改变，就会出现致命错误，比如：一个varchar类型的可能会被插入到char中，如果超长度可能会报错，而且varchar是大范围而char是小范围，可能会导致数据丢失；而如果一个浮点型的数据因为列顺序的改变插入到了一个整型的列中，这又不会报错，所以也很难找错。
- 推荐使用规范写法，特别是在开发时编写sql语句。**因为一旦指定列名，那么VALUES必须以其指定的次序匹配指定的列名，不一定按各个列出现在实际表中的次序。** 所以即使表的结构改变，此INSERT语句仍然可以正确工作。
- 第一种写法必须完整的给出表中的全部列的值，并且还得按照表中列的次序；而规范写法不用，因为会根据<table_name>(field1, field2, ...)来进行赋值，即使跟表中的列的次序不一样，或者一些列不想赋值。
---


例子：在学生表中插入数据

```mysql
INSERT INTO student(stu_id, stu_name, stu_sex)  VALUES(1, '张三', '男');
```
注意：字符串或字符需要使用''（单引号）圈起来。

输出：如果成功的话

```mysql
Query OK, 1 row affected (0.01 sec)
```

而如果在主键使用了自增长（AUTO_INCREMENT），那么则可以这样写

```mysql
INSERT INTO student(stu_id, stu_name, stu_sex)  VALUES(null, '张三', '男');
```
或者，不写主键：

```mysql
INSERT INTO student(stu_name, stu_sex)  VALUES('张三', '男');
```

也就是插入时，在主键的位置直接插入null或者不写默认就是赋给null，MySQL会因为AUTO_INCREMENT自己给它赋值。

---
- 如果对表中不允许NULL值且没有默认值的列不给出值（插入时省略了），则MySQL将产生一条错误信息，并且相应的行插入不成功。
- 规范写法哪些列可以省略不写：
    - **该列定义为允许NULL值（无值或空值）；**
    - **在表定义中给出默认值。也就表示如果在插入时不给出值，就会使用默认值；**
    - **使用AUTO_INCREMENT的列。**
- INSERT操作可能很耗时（特别是有很多索引需要更新时），而且它可能降低等待处理的SELECT语句的性能。如果数据检索是最重要的，则可以通过在INSERT和INTO之间添加关键字LOW_PRIORITY，指示MySQL降低INSERT语句的优先级。

```mysql
INSERT LOW_PRIORITY INTO
```
- UPDATE操作和DELETE操作也适用。

## 5.3 插入多个行
INSERT可以插入一行到一个表中，也可以插入多行到一个表中，第一种方式就是写多条INSERT语句，还有一种方式就是在一条INSERT语句中插入多行。

语法：

```mysql
INSERT INTO <table_name>(
    field1,
    field2,
    ...
    )
    VALUES(
        valueA1,
        valueA2,
        ...
        ),
        (
        valueB1,
        valueB2,
        ...
    );
```

此写法可以提供INSERT的性能，因为MySQL用单条INSERT语句处理多个插入比使用多条INSERT快。

## 5.4 插入检索出的数据
INSERT语句可以利用一条SELECT语句查询的结果插入到表中。也就是所谓的INSERT SELECT，它是由一条INSERT语句和一条SELECT语句构成。

比如我需要把一张表的数据复制到另一张表，语法：

```mysql
INSERT INTO <new_table_name>(field1, field2, ...) 
    SELECT field1, field2 FROM <old_table_name>;
# 或 也就是列名可以不同
INSERT INTO <new_table_name>(field1, field2, ...) 
    SELECT f1, f2 FROM <old_table_name>;
```
注意：如果新表不存在则报错。  
解释：
- 先使用SELECT语句从旧表中查询出数据，然后再使用INSERT语句把查询的结果插入到新表中。
- SELECT列出的每一个列对应于<new_table_name>后所跟的列表中的每一个列。
- 如果<old_table_name>为空，即没有行可以插入也不会报错。
- **如果能保证从旧表导入到新表（假设新表有数据）的主键值不会重复，那么插入时可以省略这主键，比如有AUTO_INCREMENT的主键就可以省略。**
- **在INSERT和SELECT语句中使用相同的列名，但是不一定要求列名匹配。实际上，MySQL甚至不关心SELECT返回的列名。它使用的是列的位置**，因此SELECT中的第一列（不管列名）将用来填充表列中指定的第一个列，第二列将用来填充表列中指定的第二列等。
- INSERT SELECT中SELECT语句可包含WHERE子句以过滤插入的数据。