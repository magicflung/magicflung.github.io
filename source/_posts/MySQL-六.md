---
title: 快速回顾MySQL：更新和删除操作
copyright: true
tags: 数据库
categories: 数据库
description: MySQL第六篇：更新和删除操作
abbrlink: 821101a1
date: 2020-01-13 12:24:11
top:
---

> 前提要述：参考书籍《MySQL必知必会》

## 6.1 更新数据
为了更新（修改）表中的数据，可使用UPDATE语句。可采用两种方式使用UPDATE：
- 更新表中特定的行；
- 更新表中所有的行。

UPDATE语法的结构由3部分组成：
- 要更新的表；
- 列名和它们的新值；
- 确定要更新行的过滤条件（WHERE关键字）。

格式：

```mysql
UPDATE <table_name> SET field1=newValue1, field2=newValue2,...
    WHERE condition;
```

解释：
- SET命令用来将新值赋给被更新的列。
- **使用WHERE子句来告诉MySQL要更新哪一行，一般会选择一个可以唯一区别其他行的字段作为条件。如果没有WHERE子句那么就会更新表中所有的行，这是非常致命的问题。**

比如：在学生表中修改李四的性别：

```mysql
UPDATE student SET stu_sex='女' WHERE stu_name='李四';
```

如果为了删除某个列的值，则可以设置为NULL（假设表定义允许NULL值）

在UPDATE语句中可以使用子查询的结果来更新数据；如果在更新一行或多行时出现一个错误，则整个UPDATE操作都会被取消（错误发生前更新的所有列被恢复到它们原来的值）。即使是错误，如果要继续执行更新，则可以使用IGNORE关键字。

```mysql
UPDATE IGNORE <table_name>...;
```

## 6.2 删除数据
为了将一个表中的某一行删除，使用DELETE语句。可以有两种方式使用DELETE:
- 从表中删除特定的行；
- 从表中删除所有行。

格式：

```mysql
DELETE FROM <table_name> WHERE condition;
```

解释：
- DELETE不需要列名和通配符。**DELETE删除整行而不是删除列**
。如果要删除列，使用UPDATE。
- **DELETE跟UPDATE一样，都必须要有WHERE，即指定要删除哪一行，所以必须注意；除非你要删除表中全部数据，那就不加WHERE。**
- WHERE一般指定的是能够唯一区分行的字段，比如主键，名字（假设没有重名的话）

例如：删除学生表中名为李四的学生：

```mysql
DELETE FROM student WHERE stu_name='李四';
```

**如果想要更快的从表中删除所有的行，不要使用DELETE，使用TRUNCATE TABLE语句，它完成相同的工作，但是速度更快。因为TRUNCATE实际是删除原来的表并重新创建一个表，而不是逐行删除表中的数据。**

## 6.3 更新和删除的指导原则
SQL程序员使用UPDATE和DELETE必须遵循的习惯：
- 除非确实打算更新和删除每一行，否则绝不要使用不带WHERE子句的UPDATE或DELETE语句。
- 保证每个表都是有主键的，尽可能像WHERE子句那样使用它（可以指定各主键，多个值或值的范围）。
- 在对UPDATE或DELETE语句使用WHERE子句前，应该先用SELECT查询语句进行测试，保证它过滤的是正确的记录，以防止编写WHERE子句不正确。
- 使用强制实施引用完整性的数据库，这样MySQL将不允许删除具有与其他表相关联的数据的行（外键）。
- MySQL没有撤销（undo）按钮，所以应该小心使用UPDATE和DELETE语句。