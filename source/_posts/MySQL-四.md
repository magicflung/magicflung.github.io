---
title: 快速回顾MySQL：数据库和表操作
copyright: true
tags: 数据库
categories: 数据库
abbrlink: c30e04ba
date: 2020-01-10 10:40:00
top:
description: MySQL第四篇：表的操作，数据库的基本操作
---

## 4.1 连接
在最初安装MySQL，可能会要求你输入一个管理登录（通常为root）和一个口令（密码）。

连接MySQL需要以下信息：
- 主机名（计算机名）——如果连接到本地MySQL服务器，为localhost；
- 端口（如果使用默认端口3306之外的端口）；
- 一个合法的用户名（默认为root）
- 用户口令（密码，如果需要的话）。

cmd登录前一篇有说。

会简单介绍Navicat Premium 12，但是后面都是执行命令，所以在Navicat Premium 12的命令行窗口做或者在cmd做都可以。

现在说下：Navicat Premium 12，打开它。

![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200110/20200110104303-263787.png)

![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200110/20200110104311-592179.png)

## 4.2 创建数据库
《MySQL必知必会》前面一开始就开讲SELETE查询等，而像新手可能就会不知道怎么创建数据库，创建表；然后利用这些表去测试所学的SELETE查询等。

下面提供两种方式创建数据库：  
1. 图形界面创建数据库

![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200110/20200110104334-611903.png)

![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200113/20200113112658-212527.png)

那么数据库就创建完成。

2. 命令行创建数据库

![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200110/20200110104407-783498.png)

![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200110/20200110104417-837863.png)

然后输入：

```
CREATE DATABASE ts;
```
更加规范的写法（推荐）：  
格式：

```
CREATE DATABASE IF NOT EXISTS <数据库名> DEFAULT CHARACTER SET='utf8';
```

比如：
```
CREATE DATABASE IF NOT EXISTS ts DEFAULT CHARACTER SET='utf8';
```
解释：
- CREATE DATABASE ts：表示创建一个名为ts的数据库；
- IF NOT EXISTS：翻译过来就是如果不存在，防止当数据库存在时会报错；
-  CREATE DATABASE IF NOT EXISTS ts：表示如果不存在一个名为ts的数据库，则创建该数据库；
-  CHARACTER SET='utf8'：这个跟我们上面图形界面设置的一样，就是设置数据库的字符集。
-  ; ：分号表示语句结束，所以每一条语句都需要分号结尾。

注意：写的时候注意中英文符号，比如 ''。

## 4.3 删除数据库
格式：

```
drop database <数据库名>;
```


## 4.4 选择数据库
可以看到我们刚刚是在my连接这里开启命令界面，我们操作的使用并不是对所有数据库进行操作，而是对单个数据库进行操作，那么就需要输入选择数据库的语句。

格式：

```
USE <数据库名>;
```

选择刚刚建立的ts数据库：

```
USE ts;
```

命令行界面会返回：

```
Database changed # 表示成功
```

## 4.5 了解数据库和表

数据库、表、用户、列、权限等信息被存储在数据库和表中（MySQL用MySQL来存储这些信息）。不过，内部的表一般不直接访问。可用MySQL的SHOW命令来显示这些信息（MySQL从内部表中提取这些信息）。

1. 查询已经存在的数据库(包括MySQL系统数据库：mysql和information_schema)：

```
SHOW DATABASES;
```
输出：  
![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200110/20200110104436-724208.png)

2. 获取一个数据库中的表的列表，前提是已经选择数据库，不然也不知道你要获取哪个数据库，假设获取mysql数据库的表。这里只是演示，一般别去动mysql数据库，毕竟是mysql的系统数据库。

```
USE mysql;
SHOW TABLES; # 显示数据库的全部表
```
输出：  
![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200110/20200110105347-683217.png)

3. 显示一个表中的列


```
USE mysql;
SHOW COLUMNS FROM 表名称;
```

输出：  
![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200110/20200110104506-805857.png)

MySQL支持用 DESCRIBE 作为 SHOW COLUMNS FROM 的一种快捷方式。


4.其他的SHOW语句：
- SHOW STATUS：用于显示广泛的服务器状态信息；
- SHOW CREATE DATABASE和SHOW CREATE TABLE：分别用来显示创建特定数据库或表的MySQL语句；
- SHOW GRANTS：用来显示授予用户（所有用户或特定用户）的安全权限；
- SHOW ERRORS和SHOW WARNINGS：用来显示服务器错误或警告消息。


## 4.6 MySQL数据类型
[转自菜鸟教程](https://www.runoob.com/mysql/mysql-data-types.html)

MySQL中定义数据字段的类型对你数据库的优化是非常重要的。

MySQL支持多种类型，大致可以分为三类：数值、日期/时间和字符串(字符)类型。

### 4.6.1 数值类型
MySQL支持所有标准SQL数值数据类型。这些类型包括严格数值数据类型(INTEGER、SMALLINT、DECIMAL和NUMERIC)，以及近似数值数据类型(FLOAT、REAL和DOUBLE PRECISION)。  

关键字INT是INTEGER的同义词，关键字DEC是DECIMAL的同义词。

BIT数据类型保存位字段值，并且支持MyISAM、MEMORY、InnoDB和BDB表。  

作为SQL标准的扩展，MySQL也支持整数类型TINYINT、MEDIUMINT和BIGINT。下面的表显示了需要的每个整数类型的存储和范围。
![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200110/20200110104519-460960.png)

### 4.6.2 日期和时间类型
表示时间值的日期和时间类型为DATETIME、DATE、TIMESTAMP、TIME和YEAR。

每个时间类型有一个有效值范围和一个"零"值，当指定不合法的MySQL不能表示的值时使用"零"值。

TIMESTAMP类型有专有的自动更新特性，将在后面描述。
![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200110/20200110104528-868714.png)

### 4.6.3 字符串类型
字符串类型指CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM和SET。该节描述了这些类型如何工作以及如何在查询中使用这些类型。
![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200110/20200110104538-243316.png)
CHAR 和 VARCHAR 类型类似，但它们保存和检索的方式不同。它们的最大长度和是否尾部空格被保留等方面也不同。在存储或检索过程中不进行大小写转换。

BINARY 和 VARBINARY 类似于 CHAR 和 VARCHAR，不同的是它们包含二进制字符串而不要非二进制字符串。也就是说，它们包含字节字符串而不是字符字符串。这说明它们没有字符集，并且排序和比较基于列值字节的数值值。

BLOB 是一个二进制大对象，可以容纳可变数量的数据。有 4 种 BLOB 类型：TINYBLOB、BLOB、MEDIUMBLOB 和 LONGBLOB。它们区别在于可容纳存储范围不同。

有 4 种 TEXT 类型：TINYTEXT、TEXT、MEDIUMTEXT 和 LONGTEXT。对应的这 4 种 BLOB 类型，可存储的最大长度不同，可根据实际情况选择。

[转自菜鸟教程评论区](https://www.runoob.com/mysql/mysql-data-types.html)：

关于 char、varchar 与 text 平时没有太在意，一般来说，可能现在大家都是用 varchar。但是当要存储的内容比较大时，究竟是选择 varchar 还是 text 呢？

这三种类型比较：

1. char:  char 不用多说了，它是定长格式的，==**但是长度范围是 0~255。 当你想要储存一个长度不足 255 的字符时，Mysql 会用空格来填充剩下的字符。因此在读取数据时，char 类型的数据要进行处理，把后面的空格去除。**== 这就是一个坑。
2. varchar:  关于 varchar，有的说最大长度是 255，也有的说是 65535，查阅很多资料后发现是这样的：++varchar 类型在 5.0.3 以下的版本中的最大长度限制为 255++，而++在 5.0.3 及以上的版本中，varchar 数据类型的长度支持到了 65535++，也就是说可以存放 65532 个字节（==注意是字节而不是字符！！！==）的数据（起始位和结束位占去了3个字节），也就是说，在 5.0.3 以下版本中需要使用固定的 TEXT 或 BLOB 格式存放的数据可以在高版本中使用可变长的 varchar 来存放，这样就能有效的减少数据库文件的大小。
3. text: 与 char 和 varchar 不同的是，text 不可以有默认值，其最大长度是 2 的 16 次方-1

总结起来，有几点：
- 经常变化的字段用 varchar
- 知道固定长度的用 char（比如性别）
- 超过 255 字符的只能用 varchar 或者 text
- 能用 varchar 的地方不用 text
- 理论上在表中的列全都可以使用varchar来定义列的数据类型，但是千万不要这样，最好根据列的实际情况选择对应的数据类型，并且可以优化数据库，比如：性别-》char，学生学号位一般用整型来存储。

[下面转载](https://blog.csdn.net/u013412790/article/details/51615407)  
数据类型的选择特别重要：
1. 数据类型会影响存储空间的开销。
2. 数据类型会影响数据库查询性能。

**所以当一个数据类型可以有多种选择多种类型的时候，应该优先考虑数字类型，其次是日期或二进制类型，最后应该是字符类型。对于相同级别的数据类型，应该优先选择占用空间小的数据类型。**  
原理：在对数据进行比较（查询条件，JOIN条件及排序）操作时：同样的数据，字符处理往往比数字处理慢，而且在数据库中，数据的处理是以页为单位，列的长度越小，数据类型占用的空间越小，利于性能的提升。

更多的优化等我看《高性能MySQL》再总结。

## 4.7 创建数据库表

创建表有两种方式：
- 使用具有交互式创建和管理表的工具（Navicat Premium 12 图形界面）；
- 表也可以直接用MySQL语句操纵。


利用CREATE TABLE创建表需要以下信息：
- 新表的名称，再关键字CREATE TABLE之后给出；
- 表列的名字和定义，用逗号隔开



创建表的语法：

```
CREATE TABLE 表名(
    列名1 列的定义比如数据类型，是否为空，是否为主键等,
    列名2 列的定义，
    ...
);
```

简单创建单表的例子：

```
CREATE TABLE student(
    stu_id INT AUTO_INCREMENT,
    stu_name varchar(10) NOT NULL,
    stu_sex char(1) NULL,
    PRIMARY KEY(stu_id)
    )ENGINE=InnoDB DEFAULT CHARSET=utf8;;
```
解释：
- 如果你不想字段为 NULL 可以设置字段的属性为 NOT NULL， 在操作数据库时如果输入该字段的数据为NULL ，就会报错。
- AUTO_INCREMENT定义列为自增的属性，一般用于主键，数值会自动加1。
- PRIMARY KEY关键字用于定义列为主键。表中的每个行必须具有唯一的主键值，也就是一个表中至少有一个主键。 可以使用多列来定义主键，列间以逗号分隔。该关键字也可以直接写在stu_id的定义中。
- ENGINE 设置存储引擎（这个先别管），CHARSET 设置编码。


额外：

> NULL和NOT NULL：NULL值就是没有值或缺值。允许NULL值的列也允许在插入行时不给出该列的值。不允许NULL值的列不接受该列没有值的行。  

> 理解NULL：不要把NULL值与空串相混淆。NULL值是没有值，它不是空串。如果指定''（两个单引号，其间没有字符），这在NOT NULL列中是允许的。空串是一个有效的值，它不是无值。NULL值用关键字NULL而不是空串指定。  

> 主键中只能使用不允许NULL值得列。允许NULL值得列不能作为唯一标识。

> AUTO_INCREMENT：被该关键字指定的列，表示每次插入数据时会对该列自动增长，通常使用它去定义主键。比如在增加一个新记录时，需要一个ID值，这ID可以任意，只要它是唯一的即可，那么最简单的就是使用下一个ID值，即当前表中最后一条记录的ID为1，那么新插入数据时，新插入的数据的ID就为2。所以就可以使用AUTO_INCREMENT自动增加ID值，我们在插入的时候也不用去理会ID是多少。  
> 如何在表中获得AUTO_INCREMENT的值，输入：  
> SELECT last_insert_id();  
> 就会返回最后一个AUTO_INCREMENT值。  
> 也可以自己修改AUTO_INCREMENT的值，只要是唯一的。

> DEFAULT：指定列的默认值，注意默认值不允许为函数，《MySQL必知必会》推荐使用默认值而不是使用NULL值。否则使用NOT NULL也可以。

[评论区说的注意事项](https://www.runoob.com/mysql/mysql-create-tables.html)


![](http://flunggg.oss-cn-shenzhen.aliyuncs.com/hexoImg/20200110/20200110104555-897640.png)

## 4.8 引擎类型

上面创建表中最后有一个 ENGINE=InnoDB。下面来解释一下：

MySQL与其他DBMS一样有一个具体管理和处理数据的内部引擎。比如当你使用CREATE TABLE语句时，该引擎具体创建表，而在你使用SELECE查询语句时或进行其他数据库处理时，该引擎在内部处理你的请求。所以它是对我们透明化的。

MySQL与其他DBMS不一样的是，MySQL具有多种引擎。这些引擎都隐藏在MySQL服务器内，全都能执行CREATE TABLE和SELECE等命令。

为什么要这么多引擎，因为它们具有各自不同的功能和特效，为不同的任务选择正确的引擎能够获得良好的功能和灵活性。

但是在CREATE TABLE时也可以忽略它，会自动使用默认引擎（很可能是MyISAM），多数SQL语句都会默认使用它。

==以下是几个必须知道的引擎==：
- **InnoBD是一个可靠的事务处理引擎，它步支持全文本搜索；**
- **MEMORY在功能等同于MyISAM，但由于数据存储在内存（不是磁盘）中，速度很快（特别适合于临时表）。**
- **MyISAM是一个性能极高的引擎，它支持全文本搜索，但不支持事务处理。**

引擎类型可以混用。但是外键不能跨引擎。混合引擎有一个很大缺陷。**外键（用于强制实施引用完整性）不能跨引擎，即使用一个引擎的表不能引用具有使用不同引擎的表的外键**。

## 4.9 删除表
格式：

```
DROP TABLE <表名> ;
```

## 4.10 更新表

语法：


```
ALTER TABLE <表名>
操作
```

比如：  
给表添加一个列：

```
ALTER TABLE student
 ADD stu_phone CHAR(11);
```

给表删除一个列：

```
ALTER TABLE student
 DROP stu_phone;
```

外键可以在创建表定义，也可以使用ALTER TABLE来定义，下面是定义外键的一种方式：

再创建一张班主任表
```
CREATE TABLE Headmaster(
    ma_id INT PRIMARY KEY AUTO_INCREMENT,
    ma_name VARCHAR(10) NOT NULL
    )ENGINE=InnoDB DEFAULT CHARSET='utf8';
```

先给学生表添加一个 班主任的列：

```
ALTER TABLE student ADD ma_id INT;
```
定义外键：与学生表相关联。

```
ALTER TABLE student
 ADD CONSTRAINT fk_student_master
 FOREIGN KEY (ma_id) REFERENCES Headmaster(ma_id);
```

 外键语法：在创建表直接加上下面这句，修改表得再加个ADD

```
CONSTRAINT <外键名，自定义>
 FOREIGN KEY (<当前表的列，比如学生表>) REFERENCES <要关联的表，比如班主任表>(<要关联的列>);
```

复杂得表结构更改一般需要手动删除过程，它涉及一下步骤：
- 用新的列布局创建一个新表；
- 使用INSERT SELECT语句（待了解）从旧表复杂数据到新表。如果有必要，可使用转换函数和计算字段；
- 校验包含所需数据的新表；
- 重命名旧表（如果确定，可以删除它）；
- 用旧表原来的名字重命名新表；
- 根据需要，重新创建触发器，存储过程，索引和外键（除外键其他待了解）。

**小心使用ALTER TABLE ：应该在改动前做一个完整的备份（模式和数据的备份）。数据库表的更改不能撤销，如果增加了不需要的列，可能不能删除它们。类似地，如果删除了不应该删除的列，可能会丢失该列中的所有数据。**

## 4.11 重命名表
语法：

```
RENAME TABLE <原表名> TO <新表名>;
```