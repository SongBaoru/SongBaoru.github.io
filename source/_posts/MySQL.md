---
title: MySQL
date: 2023-05-06 11:02:46
toc: true
cover: https://th.bing.com/th/id/R.3c9b5429b970ae83fb4ded208ce9c807?rik=VPifCoJ%2fpjqETw&riu=http%3a%2f%2fpic16.nipic.com%2f20110906%2f128199_105134374000_2.jpg&ehk=t%2f%2bs8eNL6%2b%2fAVJexV4GQnR77FSc2ZGNsU9v2lHWEueA%3d&risl=&pid=ImgRaw&r=0
categories: IT
tags:
  - MySQL
---

# 监视器

## 打开MySQL监视器

打开MySQL监视器的命令格式：

```mysql
mysql -u 用户名 -p密码
```

或者使用命令：

```mysql
mysql -u 用户名 -p
```

然后输入密码即可

## 查看字符编码

在监视器中查看MySQL中字符编码等的设置情况的命令：

```mysql
status
```

或者使用命令：

```mysql
SHOW VARIABLES LIKE 'char%'
```

## 修改密码

修改密码的命令：

```mysql
SET PASSWORD FOR root@localhost=PASSWORD('新密码')
```

## 创建用户

创建用户的命令：

```mysql
CREATE USER 新用户名 IDENTIFIED BY ‘密码’
```

用户名需要按照“用户名@主机名”的方式写

## 设置用户权限

设置用户权限的命令：

```mysql
CRANT 赋予的权限 ON 数据库名.表名 TO 用户名
```

“赋予的权限”如果是所有权限就设为“ALL”，如果仅允许SELECL和UPDATE就设置为“SELECT,UPDATE”

如果是所有数据库的所有数据表，就设置为“\*.\*”

## 退出监视器

退出监视器的命令：

```mysql
exit
```

或者使用命令：

```mysql
quit
```

# 数据库操作

MySQL的SQL语句后面需要加分号。

## 创建数据库

创建数据库的命令格式

```mysql
CREATE DATABASE 数据库名;
```

## 指定使用的数据库

```mysql
use 数据库名
```

use不是SQL语句，所以不需要输入“;”

在使用use选择数据库的状态下也能够操作其他数据库的表，如

```mysql
SELECT * FROM db2.table;
```

在没有use db2的情况下也可以执行

## 显示数据库

### 显示当前已有的数据库

显示当前已有的数据库的命令：

```mysql
SHOW DATABASES;
```

### 显示当前使用的数据库

```mysql
SELECT DATABASE();
```

## 显示数据库中的所有表

```mysql
SHOW TABLES;
```

## 删除数据库

在MySQL中，你可以使用`DROP DATABASE`语句来删除已经存在的数据库。以下是其基本语法：

```sql
DROP DATABASE database_name;
```

在这里，`database_name`是你想要删除的数据库的名称。例如，如果你想删除名为`my_database`的数据库，你可以这样做：

```sql
DROP DATABASE my_database;
```

这将删除`my_database`数据库以及其中的所有表和数据。

请注意，`DROP DATABASE`语句会永久删除数据库以及其中的所有表和数据，因此在使用之前请确保你真的想要删除该数据库。

类似地，你也可以使用`IF EXISTS`子句来避免在尝试删除不存在的数据库时出现错误，语法如下：

```sql
DROP DATABASE IF EXISTS database_name;
```

如果`database_name`存在，那么它将被删除。如果它不存在，MySQL将发出一条警告，而不是一个错误，并允许查询继续。这对于自动化脚本或者你不确定数据库是否存在时非常有用，因为它可以防止因尝试删除不存在的数据库而导致的错误。

# 数据类型

数据库表列中的数据的种类成为数据类型

## 二进制数据类型

| 数据类型 | 含义                         | 对应范围                |
| -------- | ---------------------------- | ----------------------- |
| BLOB     | 存储二进制大型对象的数据类型 | 最多可以存储65535个字节 |

## 数值型数据类型

| 数据类型  |                   含义                   |                           对应范围                           |
| :-------: | :--------------------------------------: | :----------------------------------------------------------: |
|  TINYINT  |                极小整数值                |          -128 到 127（有符号），0 到 255（无符号）           |
| SMALLINT  |                 小整数值                 |       -32768 到 32767（有符号），0 到 65535（无符号）        |
| MEDIUMINT |             中等大小的整数值             |    -8388608 到 8388607（有符号），0 到 16777215（无符号）    |
|    INT    |             常规大小的整数值             | -2147483648 到 2147483647（有符号），0 到 4294967295（无符号） |
|  BIGINT   |              非常大的整数值              | -9223372036854775808 到 9223372036854775807（有符号），0 到 18446744073709551615（无符号） |
|   FLOAT   |               单精度浮点数               |                     精度大约为 7 位小数                      |
|  DOUBLE   |               双精度浮点数               |                     精度大约为 15 位小数                     |
|  DECIMAL  | 精确的小数值，用于存储精确的数值，如货币 |              精度可达 65 位，小数部分最多 30 位              |

输入的数值型数据可以采用指数表示法输入，在指数表示法输入的情况下，“AE+B”表示“A乘以10的B次方”，例如“6.02*10的23次方”可以表示为“6.02E+23”。

## 字符串型数据类型

|  数据类型  |                             含义                             |                对应范围                |
| :--------: | :----------------------------------------------------------: | :------------------------------------: |
|    CHAR    |                     定长字符串，长度固定                     |             0 到 255 字节              |
|  VARCHAR   |                     变长字符串，长度可变                     |            0 到 65535 字节             |
|  TINYTEXT  |                       很小的文本字符串                       |             0 到 255 字节              |
|    TEXT    |                   小到中等大小的文本字符串                   |            0 到 65535 字节             |
| MEDIUMTEXT |                     中等大小的文本字符串                     |           0 到 16777215 字节           |
|  LONGTEXT  |                         大文本字符串                         |          0 到 4294967295 字节          |
|    ENUM    | 字符串对象，只能有一组预定义的值，并且值的数量不能超过 65535 |     1 或 2 字节，根据枚举值的数量      |
|    SET     |        字符串对象，可以有多个值，值的数量不能超过 64         | 1、2、3、4 或 8 字节，根据集合值的数量 |

输入的字符串如果有“'”，需要转义处理，改为“\\'”

MySQL4.1版本之后，VARCHAR和CHAR的()中指定的位数单位变为“字符”，以VARCHAR(10)为例，不管输入的是中文还是英文还是数字，最多只能保留10个字符

**VARCHAR的字符范围：**

在MySQL 5.0.3及更高版本中，`VARCHAR`可以存储最多65535字节的数据，这包括存储长度的1到2个字节。实际的最大字符串长度由最大行大小（默认为65535字节）和使用的字符集决定。

例如，如果使用utf8字符集（最多需要3字节来存储一个字符），则`VARCHAR`可以存储最多21845个字符（65535/3，舍去小数）。如果使用utf8mb4字符集（最多需要4字节来存储一个字符），则`VARCHAR`可以存储最多16383个字符（65535/4，舍去小数）。

在MySQL 5.0.2及更低版本中，`VARCHAR`可以存储最多255个字符。

## 日期和时间型数据类型

| 数据类型  |      含义      |                        对应范围                        |
| :-------: | :------------: | :----------------------------------------------------: |
|   YEAR    |      年份      |  1901 到 2155（4位时），以及1970~2069（2位时，70~69）  |
|   DATE    |      日期      |              '1000-01-01' 到 '9999-12-31'              |
|   TIME    |      时间      |              '-838:59:59' 到 '838:59:59'               |
| DATETIME  | 日期和时间组合 |     '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'     |
| TIMESTAMP |     时间戳     | '1970-01-01 00:00:01' UTC 到 '2038-01-19 03:14:07' UTC |

MySQL中的时间类型还支持小数秒，例如 `DATETIME(3)`、`TIME(3)` 或 `TIMESTAMP(3)` 可以存储精确到毫秒的时间。在括号中的数字表示小数秒的精度。

# 表操作

## 创建表

在MySQL中，你可以使用`CREATE TABLE`语句来创建一个新的表。在`CREATE TABLE`语句中，你可以指定表名，列名，列的数据类型以及任何附加的约束。

以下是一个创建新表的基本示例：

```sql
CREATE TABLE table_name (
    column1 datatype constraint,
    column2 datatype constraint,
    column3 datatype constraint,
    ....
);
```

在这个例子中，`table_name`是你要创建的表的名称，`column1`、`column2`和`column3`是列的名称，`datatype`是列的数据类型，`constraint`是任何你想在列上应用的约束。

例如，假设你想创建一个名为`students`的表，其中有`id`、`name`、`age`和`email`四个字段。你可以这样做：

```sql
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    email VARCHAR(100)
);
```

在这个例子中：

- `id`是一个整数类型的列，它自动递增并且是表的主键。
- `name`和`email`都是变长字符类型的列，最大长度为100。
- `age`是一个整数类型的列。

注意，每一行都需要以逗号结束，但是最后一行除外。最后，整个语句需要以分号结束。

数据库名、表名、列名可以使用``（反引号）括起来

输入到列中字符串的值需要用''（单引号）或者""（双引号）括起来

创建表时指定字符编码：

```mysql
CREATE TABLE 表名 (列名1 数据类型1, 列名2 数据库类型2...) CHARSET=utf8;
```

## 字段设置

### 设置主键

主键的特点：

- 没有重复的值
- 不允许输入空值（NULL）

命令格式：

在创建表时给主键字段后添加PRIMARY KEY

```
CREATE TABLE 表名 (列名1 数据类型1 PRIMARY KEY, 列名2 数据库类型2...);
```

### 设置唯一键

唯一键（unique key）的特点

- 不允许重复

- 允许输入NULL

- 在创建表时定义唯一键：

  ```sql
  CREATE TABLE table_name (
    column1 data_type,
    column2 data_type,
    UNIQUE (column1)
  );
  ```

- 在已存在的表上添加唯一键：

  ```sql
  ALTER TABLE table_name
  ADD UNIQUE (column1);
  ```

### 设置列可以自动递增

自动递增（AUTO_INCREMENT）可以应用于任何整数类型的列。`AUTO_INCREMENT`属性允许数据库自动为新记录生成一个唯一的数字。通常用于主键。

使用`AUTO_INCREMENT`属性，需要满足以下几个条件：

- 列必须被定义为`NOT NULL`，因为它必须有值。
- 列必须被定义为整数类型（例如，`INT`，`SMALLINT`，`MEDIUMINT`，`BIGINT`）。
- 每个表只能有一个`AUTO_INCREMENT`列。

示例，将`id`列定义为`AUTO_INCREMENT`：

```mysql
CREATE TABLE employees (
    id INT AUTO_INCREMENT,
    name VARCHAR(100),
    PRIMARY KEY(id)
);
```

MySQL如何设置连续递增字段的初始值：

```sql
CREATE TABLE tablename (
    id INT(11) NOT NULL AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    PRIMARY KEY (id)
) AUTO_INCREMENT=1000;
```

如果已经创建了表，并且想改变AUTO_INCREMENT的值，可以使用ALTER TABLE命令：

```sql
ALTER TABLE tablename AUTO_INCREMENT = 1000;
```

注意，不能将AUTO_INCREMENT的值设置为比当前最大值小的数，因如果这样做，MySQL将忽略此次设置并保持当前最大值。

### 设置列的默认值

在MySQL中，可以在创建表或修改表的时候设置字段的默认值。这个默认值将被用于任何未指定该列值的新行。

1. 在创建表的时候设置默认值：

```sql
CREATE TABLE tablename (
    columnname1 INT NOT NULL DEFAULT 1,
    columnname2 VARCHAR(255) NOT NULL DEFAULT 'default_value'
);
```

在这个例子中，我们创建了一个名为"tablename"的表，它有两个字段，"columnname1"和"columnname2"。对于"columnname1"，如果在插入新的行时没有指定它的值，那么它的值将默认为1。对于"columnname2"，如果在插入新的行时没有指定它的值，那么它的值将默认为'default_value'。

2. 修改已经存在的表，为字段设置默认值：

```sql
ALTER TABLE tablename 
MODIFY columnname1 INT NOT NULL DEFAULT 1,
MODIFY columnname2 VARCHAR(255) NOT NULL DEFAULT 'default_value';
```

在这个例子中，我们更改了"tablename"表中"columnname1"和"columnname2"字段的默认值。

注意，不能为NOT NULL的字段设置默认值为NULL。对于字符串类型的字段，默认值需要用引号引起来。对于日期和时间类型的字段，可以将默认值设置为CURRENT_TIMESTAMP。

### 索引（创建、删除、查看、主键索引、唯一索引、全局索引）

在MySQL中，索引是用来加速查询操作的一种数据结构。它们可以让数据库引擎快速找到表中的特定记录。如果事先在表上创建了索引，查找时就不需要对全表进行扫描，而是利用索引进行扫描。

下面是一些关于MySQL索引的基础知识：

1. **创建索引：** 创建索引可以使用 `CREATE INDEX` 命令。例如：

   ```sql
   CREATE INDEX index_name
   ON table_name (column1, column2);
   ```

   在上面的命令中，`index_name` 是索引的名称，`table_name` 是你要在其上创建索引的表的名称，`column1` 和 `column2` 是你想在其中创建索引的列。

2. **删除索引：** 删除索引可以使用 `DROP INDEX` 命令。例如：

   ```sql
   DROP INDEX index_name ON table_name;
   ```

3. **查看索引：** 如果你想看一个表的所有索引，可以使用 `SHOW INDEX` 命令。例如：

   ```sql
   SHOW INDEX FROM table_name;
   ```

4. **主键索引：** 主键（PRIMARY KEY）自动创建唯一索引。每个MySQL表只能有一个主键。

5. **唯一索引：** 唯一索引（UNIQUE INDEX）不允许任何重复值。它们可以是主键，也可以不是。

   唯一索引（Unique Index）的设置：

   - 在创建表时定义唯一索引：

     ```sql
     CREATE TABLE table_name (
       column1 data_type,
       column2 data_type,
       UNIQUE INDEX index_name (column1)
     );
     ```

   - 在已存在的表上添加唯一索引：

     ```sql
     ALTER TABLE table_name
     ADD UNIQUE INDEX index_name (column1);
     ```

6. **全文索引：** 全文索引（FULLTEXT INDEX）用于全文搜索。只有CHAR、VARCHAR和TEXT列可以创建全文索引。


使用索引的缺点：

1. 会占用额外的磁盘空间
2. 在执行插入、更新或删除操作时，索引也需要被更新，有耗时
3. 如果被创建索引的列中重复值较多，即使在该列上创建索引也不会提高处理速度

## 修改表

在MySQL中，你可以使用`ALTER TABLE`语句来修改已经存在的表。使用ALTER TABLE命令修改列的结构，根据修改类型，可以使用带有CHANGE、MODIFY、ADD、DROP的语句。

以下是一些常见的表修改操作：

1. **添加列：**

   ```sql
   ALTER TABLE table_name
   ADD column_name datatype;
   ```

   例如，如果你想在`students`表中添加一个名为`address`的新列，你可以这样做：

   ```sql
   ALTER TABLE students
   ADD address VARCHAR(255);
   ```

   把列添加到指定位置的命令格式：

   ```mysql
   ALTER TABLE 表名 ADD 列名 数据类型 AFTER 放在这个列之后;
   ```

   示例：

   ```mysql
   ALTER TABLE tb1 ADD birth DATETIME AFTER employeeId;
   ```

2. **删除列：**

   ```sql
   ALTER TABLE table_name
   DROP COLUMN column_name;
   ```

   例如，如果你想从`students`表中删除`address`列，你可以这样做：

   ```sql
   ALTER TABLE students
   DROP COLUMN address;
   ```

3. **修改列的数据类型：**

   ```sql
   ALTER TABLE table_name
   MODIFY COLUMN column_name datatype;
   ```

   即使数据类型不变，也依然需要指定修改后的数据类型

   例如，如果你想修改`students`表中`age`列的数据类型为`SMALLINT`，你可以这样做：

   ```sql
   ALTER TABLE students
   MODIFY COLUMN age SMALLINT;
   ```

4. **重命名列：**

   ```sql
   ALTER TABLE table_name
   CHANGE COLUMN old_column_name new_column_name datatype;
   ```

   例如，如果你想将`students`表中的`email`列重命名为`email_address`，你可以这样做：

   ```sql
   ALTER TABLE students
   CHANGE COLUMN email email_address VARCHAR(100);
   ```

   即使数据类型不变，也依然需要指定修改后的数据类型

5. **添加唯一键、主键或索引：**

   ```sql
   ALTER TABLE table_name
   ADD UNIQUE (column_name);
   
   ALTER TABLE table_name
   ADD PRIMARY KEY (column_name);
   
   ALTER TABLE table_name
   ADD INDEX index_name (column_name);
   ```

   `ALTER TABLE`语句会锁定表，直到操作完成。

## 显示表结构

ESC或者DESCRIBE，用于显示指定表的列结构，包括列名、数据类型、是否允许NULL，以及其他关于列的信息。

运行这个命令会返回以下信息：

- **Field**：列的名称。
- **Type**：列的数据类型和长度。
- **Null**：如果列可以包含NULL值，这个字段会显示"YES"，否则显示"NO"。
- **Key**：如果列是某种键，这个字段会显示键的类型。"PRI"表示主键，"UNI"表示唯一键，"MUL"表示这个列是一个非唯一索引，或者这个列是多个列的一部分，这些列作为复合主键或复合索引，也可能是外键。
- **Default**：列的默认值。如果没有指定默认值，这个字段会显示NULL。
- **Extra**：其他的额外信息，例如，如果列被定义为AUTO_INCREMENT，这个字段就会显示"AUTO_INCREMENT"。

，命令如下：

```mysql
DESC 表名;
```

输出示例：

```sql
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| name  | varchar(100) | YES  |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
```

## 查看创建表的SQL语句

1. SHOW CREATE TABLE语句

SHOW CREATE TABLE语句可用于查看创建表的SQL语句，包括所有列、键和约束：

```mysql
SHOW CREATE TABLE table_name;
```

返回结果示例：

```mysql
CREATE TABLE `employee` (
  `id` int NOT NULL,
  `emp_id` char(10) DEFAULT NULL,
  `emp_name` varchar(10) DEFAULT NULL,
  `manager_id` char(10) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

## 复制表

在MySQL中，你可以通过创建新表并从现有表中复制数据来复制表。下面是一种方法：

```sql
CREATE TABLE new_table AS SELECT * FROM existing_table;
```

在这里，`new_table`是你想要创建的新表的名称，`existing_table`是你想要复制数据的现有表的名称。这个操作将创建一个新表，并将现有表中的所有数据复制到新表中。

需要注意的是，使用这种方法创建的新表不会包含现有表的索引、主键、唯一键和其他约束。如果你需要复制这些属性，你需要使用其他方法，例如先使用`CREATE TABLE`语句创建表和相应的约束，然后使用`INSERT INTO ... SELECT`语句复制数据：

```sql
CREATE TABLE new_table LIKE existing_table; 
INSERT INTO new_table SELECT * FROM existing_table;
```

在这个例子中，`CREATE TABLE new_table LIKE existing_table;`语句创建一个与现有表结构（包括索引和约束）完全相同的新表，但不包含任何数据。然后，`INSERT INTO new_table SELECT * FROM existing_table;`语句将现有表中的所有数据复制到新表中。

## 复制符合条件的记录

当另一个表已经存在时，可以使用 `INSERT INTO SELECT` 语句来从一个表复制符合条件的记录到另一个表中。具体来说，`INSERT INTO SELECT` 语句会将一个查询结果插入到指定的表中。

以下是一个使用 `INSERT INTO SELECT` 语句复制符合条件的记录的示例如下：

```mysql
INSERT INTO newtable (col1, col2, ...)
	SELECT col1, col2, ...
FROM oldtable
	WHERE condition;
```

在这个例子中，我们把符合查询条件的列数据从旧表 oldtable 复制到新表 newtable。选择需要复制的列，并通过 WHERE 子句指定要复制的特定行。

需要注意的是，在执行 INSERT INTO SELECT 语句之前，我们必须先创建新表，并保证它与旧表拥有相同的结构。此外，也可以为新表添加索引或其他约束，以确保数据完整性和查询性能。

还可以在 `SELECT` 语句中使用 JOIN、GROUP BY、HAVING 等功能来实现更加复杂的查询操作。例如，我们可以使用 `JOIN` 连接多个表，并根据多个条件对记录进行筛选，最终将满足条件的记录插入到新表中。



如果需要将一个表中符合条件的记录复制到一个新表中，并且该表不存在，可以使用以下语句：

```mysql
CREATE TABLE newtable
	SELECT col1, col2, ...
FROM oldtable
	WHERE condition;
```

这条 `CREATE TABLE AS` 语句会首先创建一个名为 newtable 的新表，然后将满足 WHERE 子句指定的条件的所有行从 oldtable 复制到新表中。

需要注意的是，新表的列会自动继承 SELECT 列出的列的名称和数据类型。如果需要重新命名列或更改其数据类型，则可以使用 `AS` 或其他列定义语法来修改列属性。例如：

```mysql
CREATE TABLE newtable
	SELECT id AS new_id, name, CAST(age AS VARCHAR(10)) AS age_str
FROM oldtable
	WHERE condition;
```

在这个例子中，我们在 SELECT 语句中为新表定义了新的列名和数据类型，并将旧表的 id 列重命名为 new_id。最终，MySQL 将会根据 SELECT 语句的结果集自动创建新表的结构，并将所有符合条件的记录插入到新表中。

总而言之，`CREATE TABLE AS` 是一个强大的功能，它可以帮助我们快速创建一个新表并复制符合特定条件的记录。无论是用于数据备份、数据迁移还是进行数据分析，都是非常有用的工具。

## 删除表

在MySQL中，你可以使用 `DROP TABLE` 语句来删除已经存在的表。以下是其基本语法：

```sql
DROP TABLE table_name;
```

在这里，`table_name` 是你想要删除的表的名称。例如，如果你想删除名为 `students` 的表，你可以这样做：

```sql
DROP TABLE students;
```

此外，在目标表不存在的情况下执行DROP命令会发生错误，如果和`IF EXISTS`子句一起使用，就可以避免在试图删除不存在的表时出现错误。下面是如何使用它的语法：

```sql
DROP TABLE IF EXISTS table_name;
```

在这里，`table_name`是你想要删除的表的名称。例如，如果你想删除名为`students`的表，你可以这样做：

```sql
DROP TABLE IF EXISTS students;
```

如果`students`表存在，上述语句将删除它。如果`students`表不存在，MySQL将发出一条警告，而不是一个错误，并允许查询继续。

## 删除所有记录

请注意，`DROP TABLE` 语句会永久删除表以及表中的所有数据，因此在使用之前请确保你真的想要删除该表。如果你只是想删除表中的所有数据，但是想保留表的结构（例如列的定义和约束），你应该使用 `TRUNCATE TABLE` 语句，在MySQL中，`TRUNCATE`是一个DDL（数据定义语言）语句，用于删除表中的所有记录如下：

```sql
TRUNCATE TABLE table_name;
```

在这个例子中，`TRUNCATE TABLE students;` 会删除 `students` 表中的所有数据，但是表本身和其结构仍然存在。

或者使用如下命令也可以达到同样的效果：

```mysql
DROP FROM table_name;
```

## 查询数据

```mysql
SELECT 列名1, 列名2... FROM 表名;
```

```mysql
SELECT * FROM 表名;
```

```mysql
SELECT '会将这段文字输出'
```

```mysql
SELECT (1+2)*3;
```

## 插入记录

在MySQL中，可以使用`INSERT INTO`语句向表中插入数据。以下是几个例子：

1. **插入完整的行数据：**

   在这种情况下，你需要提供表中每个列的值。

   ```sql
   INSERT INTO table_name (column1, column2, column3, ...)
   VALUES (value1, value2, value3, ...);
   ```

   例如，如果你有一个名为`students`的表，它有`id`, `name`, `age`, `email`四个列，你可以这样插入数据：

   ```sql
   INSERT INTO students (id, name, age, email)
   VALUES (1, 'John Doe', 20, 'johndoe@example.com');
   ```

   注意：如果你的表有一个设置了`AUTO_INCREMENT`的列（如`id`列），你可以在插入数据时省略该列，MySQL会自动为它分配一个值。

2. **插入特定列的数据：**

   在这种情况下，你只需要提供你想插入的列的值。

   ```sql
   INSERT INTO table_name (column1, column2, ...)
   VALUES (value1, value2, ...);
   ```

   例如，如果你只想插入`students`表中的`name`和`email`：

   ```sql
   INSERT INTO students (name, email)
   VALUES ('John Doe', 'johndoe@example.com');
   ```

3. **插入多行数据：**

   你可以一次插入多行数据，只需在`VALUES`关键字后面列出所有的值。

   ```sql
   INSERT INTO table_name (column1, column2, ...)
   VALUES 
   (value1, value2, ...),
   (value1, value2, ...),
   ...
   ;
   ```

   例如：

   ```sql
   INSERT INTO students (name, email)
   VALUES 
   ('John Doe', 'johndoe@example.com'),
   ('Jane Doe', 'janedoe@example.com');
   ```

## 更新记录

在MySQL中，你可以使用`UPDATE`语句来更新已经存在的记录。以下是其基本语法：

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

在这里，`table_name`是你想要更新记录的表的名称，`column1`, `column2`, ... 是你想要更新的列的名称，`value1`, `value2`, ... 是你想设置的新值，`condition`是用于确定哪些记录应该被更新的条件。如果没有使用WHERE设置条件，列中的所有记录都会被替换掉，为防止这种情况发生，在启动MySQL监视器的时候，可以加上--safe-update选项，使用此选项后，如果没有WHERE条件就无法执行UPDATE或DELETE。

以下是一个例子：

```sql
UPDATE students
SET age = 21, email = 'newemail@example.com'
WHERE name = 'John Doe';
```

这个语句会在`students`表中找到所有名字为`John Doe`的记录，并将它们的`age`列设置为21，`email`列设置为`newemail@example.com`。

请注意，如果你省略了`WHERE`子句，`UPDATE`语句将更新表中的所有记录。因此，除非你真的想更新所有记录，否则应始终在使用`UPDATE`语句时包含`WHERE`子句。

另外，你应该谨慎使用`UPDATE`语句，因为一旦记录被更新，你就不能撤销更改。在更新重要的表之前，你应该考虑备份数据。

## 删除记录

在 MySQL 中，可以使用 `DELETE` 语句删除表中的记录。以下是一些常见的用法和语法：

1. 删除整个表中的所有记录

```mysql
DELETE FROM table_name;
```

这条语句将会从表 `table_name` 中删除所有记录。需要注意的是，`DELETE` 操作无法撤销，因此务必谨慎地使用。

2. 删除符合条件的记录

```mysql
DELETE FROM table_name WHERE condition;
```

这条语句将会从表 `table_name` 中删除满足指定条件的记录。例如，我们可以将下面的语句用于删除年龄大于 30 岁的人员信息：

```mysql
DELETE FROM person WHERE age > 30;
```

3. 删除部分符合条件的记录

有时候，我们可能只想删除表中的前几行数据，或者只删除满足某些条件的前几行数据。可以使用 `LIMIT` 子句来限制待删除的记录数。例如，以下语句将会删除表 `person` 中前 10 行记录：

```mysql
DELETE FROM person LIMIT 10;
```

如果要删除符合条件的前 10 行数据，则可以如下写：

```mysql
DELETE FROM person WHERE age > 30 ORDER BY id LIMIT 10;
```

在这个例子中，我们选择指定了 `WHERE` 条件和排序方式，并使用 `LIMIT` 限制了最多删除 10 行记录。

# 关键字

## WHERE

在MySQL中，`WHERE`子句用于在查询中指定条件，以过滤出满足特定条件的记录。以下是`WHERE`子句的基本语法：

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

在这里，`column1`, `column2`, ... 是你想要从表中选择的列的名称，`table_name` 是你想要查询的表的名称，`condition` 是用于确定哪些记录应该被返回的条件。

以下是一些使用`WHERE`子句的例子：

1. **基于单个条件的查询：**

    ```sql
    SELECT * FROM students WHERE age > 20;
    ```

    这个查询将返回年龄大于 20 的所有学生记录。

2. **基于多个条件的查询：**

    ```sql
    SELECT * FROM students WHERE age > 20 AND gender = 'Female';
    ```

    这个查询将返回年龄大于 20 并且性别为女性的学生记录。

3. **使用比较运算符的查询：**

    ```sql
    SELECT * FROM students WHERE age BETWEEN 18 AND 25;
    ```

    这个查询将返回年龄在 18 到 25 之间的学生记录。

4. **使用逻辑运算符的查询：**

    ```sql
    SELECT * FROM students WHERE age > 20 OR gender = 'Female';
    ```

    这个查询将返回年龄大于 20 或性别为女性的学生记录。

请注意，`WHERE`子句可以使用比较运算符（如 `=`, `<>`, `<`, `>`, `<=`, `>=`），逻辑运算符（如 `AND`, `OR`, `NOT`），以及其他条件表达式（如 `IN`, `LIKE`, `IS NULL` 等）来构建复杂的条件。

WHERE可以对GROUP BY分组前的记录进行过滤，即在如下语句中，WHERE在GROUP BY之前执行：

```mysql
SELECT
	empid, AVG(sales)
FROM
	WHERE sales >= 50
GROUP BY empid;
```

## HAVING

在MySQL中，HAVING子句只能和GROUP BY子句搭配使用，`HAVING`子句用于在`GROUP BY`子句后对分组结果进行筛选，而且只能对聚合函数进行过滤和筛选。而GROUP BY子句则是将原始数据集按照指定的列进行分组，生成多个子集，并可使用聚合函数对每个子集进行计算，HAVING是在分组后执行的。

在执行GROUP BY子句之后，可以使用HAVING子句来进一步筛选符合条件的结果集。

在MySQL中，`HAVING`子句用于在`GROUP BY`子句后对分组结果进行筛选。它允许你使用聚合函数和条件来过滤分组后的结果集。

以下是`HAVING`的基本语法：

```sql
SELECT column1, column2, ..., aggregate_function(column)
FROM table_name
GROUP BY column1, column2, ...
HAVING condition;
```

在这里，`column1`, `column2`, ... 是你想要从表中选择的列的名称，`table_name` 是你想要查询的表的名称，`aggregate_function` 是一个聚合函数（如 `SUM`, `COUNT`, `AVG` 等），`column` 是你想要按照其进行分组的列，`condition` 是用于筛选结果的条件。

以下是一个例子：

```sql
SELECT department, COUNT(*) as total_students
FROM students
	GROUP BY department
HAVING COUNT(*) > 10;
```

这个查询将根据学生表中的`department`列对记录进行分组，并计算每个部门的学生人数。然后，`HAVING`子句筛选出具有超过 10 名学生的部门。

请注意，`HAVING`子句在查询中的位置很重要，它通常出现在`GROUP BY`子句之后，`ORDER BY`子句之前。与`WHERE`子句不同，`HAVING`子句可以使用聚合函数和列的别名来指定条件。

另外，如果你只想筛选单个表中的数据而不进行分组，你可以使用`WHERE`子句。`HAVING`子句主要用于筛选分组后的结果。

## BETWEEN

在MySQL中，BETWEEN是一个比较运算符，用于检查一个值是否在指定的一组或区间内。它能够用于数字，日期和时间等数据类型。

BETWEEN语法如下：

```mysql
value BETWEEN low AND high;
```

其中，value是需要进行比较的值，low和high是指定的区间范围。对于数字类型的值来说，BETWEEN会返回一个布尔值，表示这个值是否在区间范围内；而对于日期和时间类型的值来说，BETWEEN则会将这个值转换为一个日期/时间对象后再进行比较。

例如，假设我们有一个名为`orders`的表格，其中包含了订单的信息，包括订单编号、下单日期和订单总额等。如果我们想要查询某一段时间内的订单，可以使用BETWEEN关键字来实现，如下所示：

```mysql
SELECT * FROM orders WHERE order_date BETWEEN '2021-01-01' AND '2021-03-31';
```

这条语句查询了从2021年1月1日到2021年3月31日之间的订单信息。

需要注意的是，在使用BETWEEN时，应该确保low和high的顺序正确，即low应该小于等于high。否则会导致结果不准确。同时也需要注意数据类型的匹配问题，避免类型不匹配的情况。

## IN

在MySQL中，`IN`是一个逻辑运算符，它允许你在`WHERE`或`HAVING`子句中指定多个值。以下是其基本语法：

```sql
SELECT column1, column2, ...
FROM table_name
WHERE column_name IN (value1, value2, ...);
```

在这个语句中，`column1`, `column2`, ... 是你想从表中选择的列的名称，`table_name`是你想从中选择数据的表的名称，`column_name`是你想要应用条件的列的名称，`value1`, `value2`, ... 是你想在列中查找的值。

例如，如果你想从`students`表中选择名字为'John Doe'或'Jane Doe'的学生，你可以这样做：

```sql
SELECT * FROM students WHERE name IN ('John Doe', 'Jane Doe');
```

`IN`运算符等价于多个`OR`条件。上面的查询等价于以下查询：

```sql
SELECT * FROM students WHERE name = 'John Doe' OR name = 'Jane Doe';
```

你还可以在`IN`子句中使用子查询来动态生成值的列表。例如，以下查询会选择所有在`courses`表中有记录的学生：

```sql
SELECT * FROM students WHERE id IN (SELECT student_id FROM courses);
```

在这个例子中，子查询`SELECT student_id FROM courses`会返回所有在`courses`表中有记录的学生的ID，然后主查询会从`students`表中选择这些ID对应的学生。

“=”不能代替IN，因为“=”在返回结果多于一条的时候会报错，“Subquery returns more than 1 row”。

## EXISTS

在MySQL中，`EXISTS`是一个条件运算符，用于检查一个子查询是否返回了任何行。它通常与`SELECT`语句的`WHERE`子句一起使用。

以下是`EXISTS`的基本语法：

```sql
SELECT column1, column2, ...
FROM table_name
WHERE EXISTS (subquery);
```

在这里，`column1`, `column2`, ... 是你想要从表中选择的列的名称，`table_name` 是你想要查询的表的名称，`subquery` 是一个子查询，它可以是一个完整的`SELECT`语句。

以下是一个例子：

```sql
SELECT name
FROM students
WHERE EXISTS (
    SELECT *
    FROM courses
    WHERE courses.student_id = students.id
);
```

这个查询将返回在`students`表中存在对应课程的学生的姓名。子查询检查是否存在与`students`表中的学生关联的记录在`courses`表中。

`EXISTS`运算符返回布尔值（`TRUE`或`FALSE`），如果子查询返回至少一行，则为`TRUE`，否则为`FALSE`。它通常用于`WHERE`子句中作为一个条件来过滤查询结果。

请注意，`EXISTS`子查询可以是任意复杂的查询，它可以包含聚合函数、子查询、连接等。在使用`EXISTS`时，关联子查询通常使用相关列来建立条件，以与外部查询进行关联。

## LIKE

在MySQL中，`LIKE`是一个用于模式匹配的操作符，通常用于`WHERE`子句中以过滤符合特定模式的数据。

以下是`LIKE`的基本语法：

```sql
SELECT column1, column2, ...
FROM table_name
WHERE column_name LIKE pattern;
```

在这里，`column1`, `column2`, ... 是你想要从表中选择的列的名称，`table_name` 是你想要查询的表的名称，`column_name` 是你想要应用模式匹配的列的名称，`pattern` 是匹配模式。

`LIKE`操作符使用通配符来匹配模式，常用的通配符有：

- `%`：匹配任意字符（包括零个字符）。
- `_`：匹配任意单个字符。
- `[characters]`：匹配指定字符集中的任意单个字符。
- `[^characters]`：匹配不在指定字符集中的任意单个字符。

以下是一些使用`LIKE`的例子：

1. **以特定字符开头的匹配：**

    ```sql
    SELECT * FROM students WHERE name LIKE 'J%';
    ```

    这个查询将返回名字以字母 'J' 开头的所有学生记录。

2. **以特定字符结尾的匹配：**

    ```sql
    SELECT * FROM students WHERE email LIKE '%example.com';
    ```

    这个查询将返回邮箱以 '@example.com' 结尾的所有学生记录。

3. **包含特定字符的匹配：**

    ```sql
    SELECT * FROM students WHERE name LIKE '%Doe%';
    ```

    这个查询将返回名字中包含 'Doe' 的所有学生记录。

4. **指定单个字符的匹配：**

    ```sql
    SELECT * FROM students WHERE name LIKE '_ohn';
    ```

    这个查询将返回名字为四个字符并以 'ohn' 结尾的所有学生记录，其中第二个字符可以是任意字符。

请注意，`LIKE`匹配区分大小写，如果需要不区分大小写的匹配，可以使用`COLLATE`关键字指定不区分大小写的字符集，如 `WHERE column_name COLLATE utf8_general_ci LIKE pattern;`。

## CASE WHEN

MySQL 的 CASE WHEN 是一种条件表达式，它类似于其他编程语言中的 switch 或 if-then-else 结构。它可以在 SELECT 语句中使用，根据一个或多个条件返回不同的值。

CASE WHEN 语法的基本结构如下：

```mysql
CASE expression
WHEN value_1 THEN result_1
WHEN value_2 THEN result_2
...
ELSE default_result
END
```

CASE 关键字后面的 expression 是要检查的值或表达式，value_x 是与 expression 进行比较的值，而 result_x 则是与 value_x 对应的结果。如果 expression 和某个 value_x 相匹配，则会返回对应的 result_x。如果都没有匹配，将会执行 ELSE 子句中指定的 default_result（可选的，如果没有 ELSE 子句，将返回 NULL）。

以下是一个简单的例子，演示了如何在 SELECT 语句中使用 CASE WHEN：

```mysql
SELECT employee_name, 
CASE department_id
	WHEN 1 THEN 'Sales'
    WHEN 2 THEN 'Marketing'
    ELSE 'Other'
END as department
FROM employees;
```

这条语句查询了一个名为 employees 的表格，其中包含员工信息，包括姓名、部门编号等。它使用 CASE WHEN 结构来把部门编号转换为对应的文本描述，最终会返回每个员工的名称和所在部门的文本标签。

需要注意的是，CASE WHEN 也支持复杂的判断逻辑和多个条件，可以使用嵌套和逻辑运算符来实现复杂的条件判断。

以下是CASE WHEN的另一个例子：

```mysql
SELECT id,
CASE
	WHEN sales >= 100 THEN '高'
	WHEN sales >= 50 THEN '中等'
    ELSE '低'
END AS '评价'
FROM tb;
```

## IF ELSE

在MySQL中，可以使用`IF`和`THEN`关键字来实现条件语句。基本语法如下所示：

```mysql
IF condition THEN
    statement(s);
ELSE
    statement(s);
END IF;
```

其中，如果“condition”（条件）为真，则执行`THEN`子句中的一个或多个语句，否则执行`ELSE`子句中的一个或多个语句。ELSE`块是可选的。

让我们看一个实际的例子：
```mysql
SET @score = 80;

IF @score >= 60 THEN
    SELECT 'Pass';
ELSE
    SELECT 'Fail';
END IF;
```

在上述示例中，将变量`@score`设置为80分。然后，使用`IF`语句检查是否及格（分数大于等于60）。如果成立，则输出“Pass”，否则输出“Fail”。

## ORDER BY和ACS或DESC

在MySQL中，你可以使用`ORDER BY`子句对查询结果进行排序。`ORDER BY`子句可以指定按照一个或多个列进行升序（默认）或降序排序。

以下是`ORDER BY`子句的基本语法：

```mysql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC], ...;
```

在这里，`column1`, `column2`, ... 是你想要排序的列的名称，`ASC`表示升序（默认），`DESC`表示降序。

以下是一些例子：

1. **按照单个列进行升序排序：**

    ```mysql
    SELECT * FROM students
    ORDER BY name ASC;
    ```

    这将按照`name`列的字母顺序对`students`表中的记录进行升序排序。

2. **按照单个列进行降序排序：**

    ```mysql
    SELECT * FROM students
    ORDER BY age DESC;
    ```

    这将按照`age`列的逆序（从高到低）对`students`表中的记录进行降序排序。

3. **按照多个列进行排序：**

    ```mysql
    SELECT * FROM students
    ORDER BY age ASC, name ASC;
    ```

    这将首先按照`age`列进行升序排序，然后对于具有相同`age`值的记录，按照`name`列的字母顺序进行升序排序。

请注意，`ORDER BY`子句应该在`SELECT`语句的最后使用。如果你想在查询结果中以相反的顺序排序，只需在列名后使用`DESC`关键字。默认情况下，排序是升序的（ASC）。

ORDER BY可以实现对分组后的结果进行排序，MySQL语句中先写“GROUP BY”再写“ORDER BY”，示例如下：

```mysql
SELECT
	empid, AVG(sales)
FROM tb
	GROUP BY empid
ORDER BY AVG(sales)
	DESC;
```

```mysql
SELECT empid, AVG(sales)
	FROM tb
GROUP BY empid
	HAVING AVG(sales) > 50
ORDER BY AVG(sales)
	DESC;
```

## LIMIT

在MySQL中，`LIMIT`用于限制查询结果的数量。它可以在`SELECT`语句中用于指定返回的行数。

以下是`LIMIT`的基本语法：

```sql
SELECT column1, column2, ...
FROM table_name
LIMIT number_of_rows;
```

在这里，`column1`, `column2`, ... 是你想要从表中选择的列的名称，`table_name` 是你想要查询的表的名称，`number_of_rows` 是你希望返回的行数。

以下是一些使用`LIMIT`的例子：

1. **限制结果集的行数：**

    ```sql
    SELECT * FROM students LIMIT 10;
    ```

    这个查询将返回 `students` 表中的前 10 行记录。

2. **指定起始位置和行数：**

    ```sql
    SELECT * FROM students LIMIT 5, 10;
    ```

    这个查询将从 `students` 表中的第 6 行开始（偏移量为 5），返回后续的 10 行记录。这是通过使用两个参数来实现的，第一个参数是起始位置的偏移量，第二个参数是要返回的行数。

`LIMIT`语句还可以与`ORDER BY`子句一起使用，以在指定排序顺序的基础上限制结果集的行数。例如：

```sql
SELECT * FROM students ORDER BY age DESC LIMIT 5;
```

这个查询将按照年龄降序排序，并返回年龄最大的前 5 条学生记录。

请注意，`LIMIT`子句在查询中的位置很重要，它通常出现在`SELECT`语句的末尾。

## OFFSET

在 MySQL 中，`OFFSET` 用于从查询结果的某个特定行开始返回数据。`OFFSET` 必须和 `LIMIT` 一起使用，以便明确指定要返回的行数和偏移量。

`OFFSET` 的语法如下：

```mysql
SELECT column1, column2, ...
FROM table
WHERE condition
ORDER BY column1, column2, ...
LIMIT offset, count;
```

另一种语法是

```mysql
SELECT column1, column2, ...
FROM table
WHERE condition
ORDER BY column1, column2, ...
LIMIT count OFFSET offset;
```

其中，`offset` 是要偏移的行数，`count` 是要返回的行数。例如，如果希望跳过前 5 行并获取接下来的 10 行，可以这样写：

```mysql
SELECT *
FROM mytable
ORDER BY id
LIMIT 5, 10;
```

或写为：

```mysql
SELECT *
FROM mytable
ORDER BY id
LIMIT 10 OFFSET 5;
```

这个查询将返回表中排序后的第 6 行到第 15 行，因为它跳过前 5 行（即偏移量为 5），然后返回接下来的 10 行。

需要注意的是，在处理大数据集时， OFFSET 可能会影响查询性能，因为它需要跳过指定数量的行，直到达到指定偏移量。为了避免此类问题，可以考虑优化查询或者通过其他手段减少数据集的大小。

## GROUP BY

在MySQL中，`GROUP BY`子句用于将查询结果按照一个或多个列进行分组。它常与聚合函数（如`SUM`、`COUNT`、`AVG`等）一起使用，以对每个组应用聚合函数并生成汇总结果。

以下是`GROUP BY`的基本语法：

```sql
SELECT column1, column2, ..., aggregate_function(column)
FROM table_name
GROUP BY column1, column2, ...;
```

在这里，`column1`, `column2`, ... 是你想要从表中选择的列的名称，`table_name` 是你想要查询的表的名称，`aggregate_function` 是一个聚合函数（如 `SUM`, `COUNT`, `AVG` 等），`column` 是你想要按照其进行分组的列。

以下是一个例子：

```sql
SELECT department, COUNT(*) as total_students
FROM students
GROUP BY department;
```

这个查询将根据学生表中的`department`列对记录进行分组，并计算每个部门的学生人数。

请注意，`GROUP BY`子句通常与聚合函数一起使用。在`SELECT`语句中，你可以选择其他非聚合列，但这些列必须出现在`GROUP BY`子句中或作为聚合函数的参数。否则，将出现错误。

此外，你还可以在`GROUP BY`子句中使用多个列，以按照多个列进行分组。例如：

```sql
SELECT department, gender, COUNT(*) as total_students
FROM students
GROUP BY department, gender;
```

这个查询将根据`department`和`gender`两个列对记录进行分组，并计算每个部门和性别的学生人数。

## DISTINCT

MySQL中的`DISTINCT`关键字用于去重查询结果集。

例如，当我们需要查询某张表中的所有员工身份证号码时，可能会出现一些员工重复的情况。此时可以使用`DISTINCT`关键字来消除重复项，确保查询结果唯一：

```mysql
SELECT DISTINCT id_number FROM employee;
```

上述语句将返回一张包含所有不重复身份证号码的数据表。

需要注意的是，`DISTINCT`操作是在内存中进行的，如果查询的结果集比较大，或者查询条件过多，可能会影响查询性能。同时，在使用`DISTINCT`关键字时，也要注意它适用于单个列的去重，若需要多列的去重，则需要使用`GROUP BY`语句来实现。

## EXPLAIN

在MySQL中，可以使用`EXPLAIN`关键字来分析SQL语句的执行计划，从而优化查询性能。

`EXPLAIN`语法如下：

```mysql
EXPLAIN SELECT * FROM table_name WHERE condition;
```

`EXPLAIN`会返回一张表格，其中包含了关于MySQL如何处理SQL语句的信息，例如表的读取顺序、使用的索引名及类型、是否需要临时表等等。通过检查这些信息，可以找到潜在的性能问题，并针对性地进行调整。

常见的一些列含义：

- id: 执行SELECT查询的序列号，id相同表示执行相同的查询。
- select_type: 查询类型，例如Simple表示最简单的查询类型，Derived表示一个派生表或子查询中的查询类型。
- table: 指出MySQL将从哪个表或派生表读取行。 
- partitions: 匹配的分区列表，如果没有分区则为NULL。
- type: 显示表的连接类型，包括system, const, eq_ref, ref, range, index和ALL。一般来说，查询类型越复杂，性能越低。
- possible_keys: 展示MySQL可能使用哪些索引来查找表中的行，是一个逗号分隔的索引列表。 
- key: 表示实际将使用哪个索引，如果为NULL则说明没有找到合适的索引。
- key_len: 表示MySQL用了索引的长度，如果key为NULL，则key_len也为NULL。
- ref: 显示索引的哪一列被使用了，如果可能的话，可能是一个常数。 
- rows: MySQL认为它执行查询时必须检查的行数，这个值是估计出来的，并不一定准确。 
- filtered: 表示此表过滤器的百分比，即选择性的值。
- Extra: 其他信息，通常提供有关查询策略的特殊说明。

通过对`EXPLAIN`结果的分析，可以确定哪些操作需要优化，例如加上索引、优化查询条件等，从而提高查询性能。

# IN和EXIST的区别与联系

在MySQL中，`EXISTS`和`IN`是两个用于条件判断和子查询的关键字。

`EXISTS`用于检查子查询是否返回任何行，它返回一个布尔值。如果子查询返回至少一行，`EXISTS`条件为真（true），否则为假（false）。

`IN`用于判断一个值是否存在于子查询的结果集中，它也返回一个布尔值。如果值存在于子查询的结果集中，`IN`条件为真（true），否则为假（false）。

以下是使用`EXISTS`和`IN`的基本语法示例：

1. 使用`EXISTS`：
```sql
SELECT column1, column2, ...
FROM table_name
WHERE EXISTS (subquery);
```
例子：
```sql
SELECT name
FROM students
WHERE EXISTS (
    SELECT *
    FROM courses
    WHERE courses.student_id = students.id
);
```

2. 使用`IN`：
```sql
SELECT column1, column2, ...
FROM table_name
WHERE column_name IN (subquery);
```
例子：
```sql
SELECT name
FROM students
WHERE id IN (
    SELECT student_id
    FROM courses
);
```

需要注意的是，`EXISTS`通常用于检查子查询的存在性，而`IN`通常用于检查一个值是否存在于另一个查询结果集中。在某些情况下，`EXISTS`和`IN`可以互相替代，但根据具体情况选择合适的关键字会更有效。此外，`EXISTS`通常在子查询的结果集很大时具有更好的性能，而`IN`适用于较小的结果集。

# 聚合函数

1. SUM：计算某个字段的总和。
2. COUNT：返回某个字段的行数，可以用于计算某个列或整个表中行的数量。
3. AVG：计算某个字段的平均值。
4. MIN：找出某个字段的最小值。
5. MAX：找出某个字段的最大值。
6. GROUP_CONCAT：将某个字段的值连接成一个字符串，并以逗号分隔。

以下是MySQL聚合函数的基本用法及语法格式：

1. SUM

```
SELECT SUM(column_name) FROM table_name;
```

2. COUNT

```
SELECT COUNT(column_name) FROM table_name;
```

3. AVG

```
SELECT AVG(column_name) FROM table_name;
```

4. MIN

```
SELECT MIN(column_name) FROM table_name;
```

5. MAX

```
SELECT MAX(column_name) FROM table_name;
```

6. GROUP_CONCAT

```
SELECT GROUP_CONCAT(column_name SEPARATOR ',') FROM table_name;
```

其中，`column_name`为需要进行计算或处理的字段名，`table_name`为数据表的名称。需要注意的是，在使用GROUP_CONCAT时，可以指定`SEPARATOR`来指定连接字符串的分隔符，如果没有指定，默认使用逗号作为分隔符。

# 字符串操作函数

## CONCAT()

CONCAT函数是用来连接字符串的。

CONCAT函数接受两个或多个参数，并返回这些参数连接后的字符串结果。常见的用法是将两个或多个字段的值拼接成一个新的字符串：

```
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM users;
```

上面的语句会查询`users`表中的`first_name`和`last_name`字段，并将这两个字段的值用空格连接起来，作为新的列`full_name`的值进行返回。

需要注意的是，在使用CONCAT函数时，需要注意参数的顺序以及字符串之间的分隔符。例如，如果要在两个字符串之间添加空格，可以使用空格字符' '作为参数之一。

## RIGHT()

RIGHT函数返回一个字符串的右侧指定长度的子字符串。使用时需要指定两个参数：要截取的字符串和需要返回的字符长度。

例如，下面的语句会查询`users`表中的`username`字段，并返回该字段后两个字符组成的新的字符串：

```mysql
SELECT RIGHT(username, 2) AS last_two_chars FROM users;
```

## SUBSTRING()

SUBSTRING函数可以从一个字符串中截取指定位置和长度的子字符串。使用时需要指定三个参数：要截取的字符串、开始的位置（从1开始计数）和需要返回的字符长度。

例如，下面的语句会查询`users`表中的`phone_number`字段，并返回该字段第4个字符开始的3个字符组成的新的字符串：

```mysql
SELECT SUBSTRING(phone_number, 4, 3) AS area_code FROM users;
```

## REPEAT()

REPEAT函数返回一个重复指定次数的字符串。使用时需要指定两个参数：需要重复的字符串以及需要重复的次数。

例如，下面的语句会返回一个由5个“-”字符组成的新的字符串：

```mysql
SELECT REPEAT('-', 5) AS line FROM users;
```

## REVERSE()

REVERSE函数返回一个字符串的反转形式，即将字符串中的字符顺序颠倒。使用时只需要指定一个参数，即要翻转的字符串。

例如，下面的语句会查询`users`表中的`username`字段，并将该字段的字符顺序全部颠倒：

```mysql
SELECT REVERSE(username) AS reversed_name FROM users;
```

# 日期和时间函数

MySQL提供了很多日期和时间函数，可以方便地对日期和时间进行处理和计算。以下是一些常用的MySQL日期和时间函数：

## NOW()

NOW()函数返回当前日期和时间。

例如，下面的语句会查询当前日期和时间：

```mysql
SELECT NOW();
```

#### DATE()

DATE函数从一个日期或日期时间表达式中提取日期部分。

例如，下面的语句会返回当前日期：

```mysql
SELECT DATE(NOW());
```

#### TIME()

TIME函数从一个日期或日期时间表达式中提取时间部分。

例如，下面的语句会返回当前时间：

```mysql
SELECT TIME(NOW());
```

#### YEAR()

YEAR函数从一个日期或日期时间表达式中提取年份部分。

例如，下面的语句会返回当前年份：

```mysql
SELECT YEAR(NOW());
```

#### MONTH()

MONTH函数从一个日期或日期时间表达式中提取月份部分。

例如，下面的语句会返回当前月份：

```mysql
SELECT MONTH(NOW());
```

#### DAY()

DAY函数从一个日期或日期时间表达式中提取日份部分。

例如，下面的语句会返回当前日期中的天数：

```mysql
SELECT DAY(NOW());
```

#### HOUR()

HOUR函数从一个日期或日期时间表达式中提取小时部分。

例如，下面的语句会返回当前时间中的小时数：

```mysql
SELECT HOUR(NOW());
```

#### MINUTE()

MINUTE函数从一个日期或日期时间表达式中提取分钟部分。

例如，下面的语句会返回当前时间中的分钟数：

```mysql
SELECT MINUTE(NOW());
```

#### SECOND()

SECOND函数从一个日期或日期时间表达式中提取秒部分。

例如，下面的语句会返回当前时间中的秒数：

```mysql
SELECT SECOND(NOW());
```

## DATE_SUB()

在 MySQL 中，可以使用 DATE_SUB() 函数和 INTERVAL 子句来查询一个日期字段 c 为五年前的记录。具体语法如下：

```mysql
SELECT * FROM table
WHERE c = DATE_SUB(NOW(), INTERVAL 5 YEAR);
```

这个查询语句中，`NOW()` 函数返回当前时间，`DATE_SUB()` 函数将当前时间减去 5 年，然后查询满足条件的所有记录。

## CURDATE()

`CURDATE()` 函数返回系统当前日期。例如，以下查询语句将会返回当前日期：

```mysql
SELECT CURDATE();
```

## DATEDIFF()

`DATEDIFF(date1, date2)` 函数返回两个日期之间的天数差值（即 date1 - date2）。例如，以下查询语句将会计算出今天与指定日期之间的天数差值：

```mysql
SELECT DATEDIFF(CURDATE(), '2022-01-01');
```

## DATE_ADD()

`DATE_ADD(date, INTERVAL value unit)` 函数可以对给定日期增加或减去一定的时长。其中 `date` 为基准日期，`value` 为增加或减少的数值，`unit` 为时间单位（如 DAY、WEEK、MONTH、YEAR 等）。例如，以下查询语句将会计算出五年前的日期：

```mysql
SELECT DATE_ADD(CURDATE(), INTERVAL -5 YEAR);
```

## DATE_FORMAT()

DATE_FORMAT(date, format)` 函数可以将给定日期格式化成指定的字符串形式。其中 `date` 为待格式化的日期，`format` 为日期格式串（如 '%Y-%m-%d' 表示以年、月、日的格式显示日期）。例如，以下查询语句将会格式化出当前日期并输出：

```mysql
SELECT DATE_FORMAT(CURDATE(), '%Y-%m-%d');
```

# 基于条件的查询

用于创建条件的关键字：

- WHERE
- HAVING

用于创建条件的关键字：

- 比较运算符（如`=`，`!=`，`<`，`>`，`<=`，`>=`，`<>`）

- 逻辑运算符（如`AND`，`OR`，`NOT`），`IN`，`EXISTS`，`BETWEEN`，`LIKE`，`IS NULL`等

  在 MySQL 中，AND、OR 和 NOT 是逻辑运算符，用于连接和组合多个条件表达式。当它们同时出现在一个查询语句中时，需要考虑它们的优先级。

  MySQL 中的运算符优先级遵循以下顺序（由高到低）：

  1. NOT
  2. AND
  3. OR

  这意味着，NOT 的优先级最高，所以它会优先计算其后面的条件表达式，然后再应用其他的运算符。而 AND 的优先级比 OR 高，因此，在一个查询语句中，如果同时出现了 AND 和 OR 运算符，AND 会先于 OR 进行计算。

  为了避免运算符优先级带来的不必要的困惑，我们可以使用圆括号来明确指定某些条件的计算顺序。具体来说，使用括号可以将几个条件组合在一起，并确保它们在适当的顺序下进行计算。

  下面是一个例子，演示了如何在查询语句中使用括号控制条件表达式的计算顺序：

  ```mysql
  SELECT * FROM mytable
  WHERE (col1 = 'value1' OR col2 = 'value2')
  AND col3 > 100;
  ```

  在这个例子中，首先使用括号指定 OR 条件的计算顺序，然后再结合 AND 运算符对这些条件进行组合。它的作用相当于检索 col1 等于 'value1'，或者 col2 等于 'value2'，并且 col3 大于 100 的所有行。

# 使用多个表

## 联合查询（UNION）

在 MySQL 中，`UNION` 关键字用于将两个或多个 `SELECT` 语句的结果集合并成一个结果集。`UNION` 可以去除重复行，而 `UNION ALL` 则包含所有的行（包括重复行）。以下是 `UNION` 和 `UNION ALL` 的语法格式：

```mysql
-- UNION 示例
SELECT column1, column2, ... FROM table1
UNION
SELECT column1, column2, ... FROM table2;
UNION
SELECT column1, column2, ... FROM table3;
...

-- UNION ALL 示例
SELECT column1, column2, ... FROM table1
UNION ALL
SELECT column1, column2, ... FROM table2;
UNION ALL
SELECT column1, column2, ... FROM table3;
...
```

在这个示例中，我们可以通过 `UNION` 合并两个表的查询结果，并返回所有不重复的行；或者通过 `UNION ALL` 合并两个表的查询结果，并返回所有行，包括重复行。

需要注意的是，在使用 `UNION` 或 `UNION ALL` 进行结果集合并时，两个 `SELECT` 语句必须满足以下条件：

- SELECT 语句中列数必须相同。
- 列类型必须相似或可隐式转换。
- 结果集中的列名来自第一个 SELECT 语句，后续的 SELECT 语句只能：
  - 与第一个 SELECT 语句中的列具有相同的名称，或者
  - 为其列命名以匹配结果集的结构。

例如，以下是一个 `UNION` 操作的示例：

```mysql
SELECT name, age FROM table1
UNION
SELECT name, age FROM table2;
UNION
SELECT name, age FROM table3;
```

这里我们将查询结果按照 `name` 和 `age` 两列进行合并，如果存在相同的行则只返回一次。需要注意的是，`UNION` 操作会对结果集进行排序和去重操作，因此可能需要-根据实际情况调整查询语句的顺序和筛选条件。

总而言之，在 MySQL 中使用 `UNION` 和 `UNION ALL` 可以使我们方便地将多个查询结果组合在一起，并可以选择是否保留重复行。

## 内连接（INNER JOIN）

INNER JOIN操作用于连接两个表并返回匹配的行，只有当在关联列中两个表都存在匹配时才会返回结果。除此之外，还有一种相似的语法格式叫做“JOIN...ON...”，其效果与INNER JOIN完全相同。

语法格式：

```mysql
SELECT column_name(s) FROM table1 
INNER JOIN table2 
ON table1.column_name = table2.column_name;
```

使用示例：
查询商品表和订单表中有关联的数据

```mysql
SELECT orders.id, orders.order_date, products.product_name
FROM orders
INNER JOIN products
ON orders.product_id = products.id;
```

在上面的例子中，我们连接了“商品”表和“订单”表，并使用ON子句指定了连接条件（“product_id”），结果集包括了这两个表中符合连接条件的数据。

## ON和USING

MySQL ON是一种在两个表之间进行连接的常用方法，通过指定连接条件来连接两个表并返回符合条件的结果。

语法格式：

```
SELECT column_name(s) FROM table1 JOIN table2 ON table1.column_name = table2.column_name;
```

其中，table1和table2是要连接的表，column_name是连接两个表的共同列名。

使用示例：
查询所有部门和员工数据

```
SELECT departments.department_name, employees.first_name, employees.last_name
FROM departments
JOIN employees ON departments.department_id = employees.department_id;
```

需要注意的是，在使用ON子句时，可以比较不同类型的数据，如数字、文本等。在连接多个表时，也可以在ON子句中使用逻辑运算符（例如AND和OR），以指定更复杂的连接条件。

MySQL USING是一种简化JOIN操作的方法，该操作将在两个表之间使用共同的列进行连接。这样可以避免在ON子句的连接条件中重复指定相同列名，从而使查询更加简洁。

语法格式：

```mysql
SELECT column_name(s) FROM table1 JOIN table2 USING (column_name);
```

其中，table1和table2是要连接的表，column_name是连接两个表的共同列名。

使用示例：
查询所有部门和员工数据

```mysql
SELECT departments.department_name, employees.first_name, employees.last_name
FROM departments
JOIN employees 
USING (department_id);
```

需要注意的是，USING只能用于比较两个表中具有相同名称的列，如上面的例子中的department_id。如果两个表的连接条件的列名称不同，则必须使用ON子句来指定要连接的列

## 外连接

#### 左外连接（LEFT JOIN）

LEFT JOIN操作连接左侧的表和右侧的表，并返回左侧表的所有记录以及满足连接条件的右侧表的记录，如果没有匹配的行，则该结果集中的右侧表的字段将被设置为NULL值。

LEFT JOIN也可以写成LEFT OUTER JOIN

语法格式：

```mysql
SELECT column_name(s) FROM table1 
LEFT [OUTER] JOIN table2 
ON table1.column_name = table2.column_name;
```

使用示例：
查询所有的部门和员工数据以及那些没有员工的部门

```mysql
SELECT departments.department_name, employees.first_name, employees.last_name
FROM departments
LEFT JOIN employees
ON departments.department_id = employees.department_id;
```

在上面的例子中，我们连接了“部门”和“员工”两个表，并使用ON子句指定了共同字段（“department_id”），因此返回的结果集包括了所有的部门和符合连接条件的员工信息，即使某些部门没有员工也会显示出来。

#### 右外连接（RIGHT JOIN）

RIGHT JOIN操作连接右侧的表和左侧的表，并返回右侧表的所有记录以及满足连接条件的左侧表的记录。如果没有匹配的行，则该结果集中的左侧字段将被设置为NULL值。

RIGHT JOIN也可以写成RIGHT OUTER JOIN

语法格式：

```mysql
SELECT column_name(s) FROM table1 
RIGHT [OUTER] JOIN table2 
ON table1.column_name = table2.column_name;
```

使用示例：
查询所有的员工数据以及那些没有被分配到部门的员工

```mysql
SELECT employees.first_name, employees.last_name, departments.department_name
FROM employees
RIGHT JOIN departments
ON employees.department_id = departments.department_id;
```

在上面的例子中，我们连接了“员工”和“部门”两个表，并使用ON子句指定了共同字段（“department_id”），因此返回的结果集包括了所有的员工信息和与之相关的部门信息，即使某些员工没有分配到部门也会显示出来。

LEFT JOIN和RIGHT JOIN可以用于处理一对多、多对一的关系查询

## 自连接

自连接是将表与其自身同名的表进行连接

在MySQL中，使用自连接时需要使用别名（alias）来区分每个表实例。以下是一个示例：

```mysql
SELECT a.employee_name, b.employee_name
FROM employee a, employee b
WHERE a.manager_id = b.employee_id;
```

在上面的例子中，我们查询了“employee”表中所有员工的名称和他们各自的经理的名称。为此，我们需要对“employee”表进行自连接，以将每个员工与其经理联系起来。这里我们使用a和b两个别名来表示同一个表中的两个不同实例。在上面的例子中，我们在WHERE子句中指定连接条件“manager_id = b.employee_id”，这意味着我们正在连接的是一个员工和他的经理。

或者也可以使用内连接关键字进行自连接：

```mysql
SELECT a.emp_id, b.emp_name AS manager_name
FROM employee a
JOIN employee b
ON a.manager_id = b.emp_id;
```

## 子查询

通俗的讲，在SELECT的记录中SELCT就是子查询。子查询是在查询中使用一个查询作为另一个查询的子集。使用子查询可以轻松地扩展查询功能并实现更复杂的条件过滤机制。

下面是一个示例，该示例演示如何使用子查询：

假设您有两个表：Orders和Customers，分别存储订单和客户的信息。现在，假设您想要查询所有来自某些城市的顾客的订单详细信息。您可以使用以下查询：

```mysql
SELECT *
FROM Orders
WHERE customer_id IN (
  SELECT customer_id
  FROM Customers
  WHERE city = 'New York'
);
```

这个查询中，内部SELECT语句是一个子查询，它返回所有位于“New York”城市的客户ID。外部SELECT语句使用WHERE子句来过滤Orders表中包含在子查询结果集中的customer_id值的行，并将行返回到结果集中。

下面是另一个示例：

提取大于等于平均值的记录

```mysql
SELECT *
FROM tb1
WHERE age >= (
	SELECT AVG(age) FROM tb1
)
```

下面是使用IN的子查询的语法：

```mysql
SELECT column_name1, column_name2... 
FROM table_name
WHERE column_name IN (通过子查询SELECT语句提取的列)
```

示例：

```mysql
SELECT empid, name
	FROM tb1
WHERE empid 
	IN (SELECT empid FROM tb2);
```

下面是使用EXISTS的子查询的语法：

```mysql
SELECT column_name1, column_name2... 
FROM table_name
WHERE EXISTS (通过子查询SELECT语句提取的列);
```

示例：

```mysql
SELECT *
	FROM tb1
WHERE EXISTS
	(SELECT * FROM tb2 WHERE tb1.empid=tb2.empid);
```

# 视图

> 将SELECT结果像表一样保存下来的虚表就是视图
>
> 任何更改基本表的操作（如INSERT、UPDATE或DELETE）都会影响到视图的结果
>
> 视图也可以和表一样进行SELECT、INSERT、UPDATE、DELETE......
>
> 视图可以帮助简化复杂查询，提高查询性能

## 创建视图

使用CREATE VIEW语句可以在MySQL中创建视图。例如，以下是创建一个简单视图的示例：

```mysql
CREATE VIEW my_view AS
SELECT column1, column2, ...
FROM my_table
WHERE condition;
```

在这个例子中，my_view是视图的名称，my_table是视图所依赖的基本表，SELECT查询表示视图的内容，并使用WHERE子句进行条件过滤。

## 可更新视图

在MySQL中，只有可更新视图（updatable view）才能使用UPDATE、INSERT或DELETE语句来更改基表的行数据。

当满足以下条件之一时，视图将被标记为不可更新：

1. 视图包含聚合函数（如SUM或AVG）。
2. 视图中使用了DISTINCT、GROUP BY或HAVING子句。
3. 视图中的SELECT语句包含UNION或UNION ALL操作符。
4. 视图定义中存在子查询，而且子查询引用了与SELECT语句所引用的不同表。
5. 视图定义中存在常量或表达式，而不是列名。

对视图执行INSERT操作时，即使与创建视图时的WHERE条件不匹配，数据也会插入到基表中。

比如创建视图时指定了sales的范围：

```mysql
CREATE VIEW v1 AS
SELECT empid, sales
FROM tb
WHERE sales >= 100;
```

INSERT数据时不在这个范围内：

```mysql
INSERT INTO v1 VALUES('new_empid', 15);
```

执行之后，虽然视图v1中没有('new_empid', 15)这条数据，但是基本表中已经INSERT成功，存在了这条数据。

为避免这种情况发生，可以将视图设置为”不接受与条件不匹配的记录“，在CREATE VIEW时，添加WITH CHECK OPTION：

```mysql
CREATE VIEW v1 AS
SELECT empid, sales
FROM tb
WHERE sales >= 100
WITH CHECK OPTION;
```

这样就无法插入不符合条件的记录了。

## 显示视图

表和视图在显示上的操作是相同的，也是通过SHOW TABLES命令操作，视图会与表一起显示出来。

要查看视图的列结构也是和表一样的操作

显示视图view_name的列结构示例如下：

```mysql
DESC view_name;
```

显示创建语句视图的SQL语句示例如下：

```mysql
SHOW CREATE VIEW view_name
```

## 替换视图

这里的替换视图的含义是删除已经存在的同名视图并创建新视图，操作方法是加上OR REPLACE：

```mysql
CREATE OR REPLACE VIEW v1 AS
SELECT NOW();
```

## 修改视图结构

修改视图结构使用的也是ALTER命令，命令格式：

```mysql
ALTER VIEW view_name AS (SELECT语句);
```

## 删除视图

如果存在就删除视图：

```mysql
DROP VIEW IF EXISTS v1;
```

IF EXISTS的作用参见删除数据库。

# 存储过程

> MySQL存储过程（Stored Procedure）是一组预编译的SQL语句集合，可以在数据库中重复运行使用
>
> 将多个SQL语句组合成一个只需要使用命令”CALL * *“就能执行的集合，该集合就称为存储过程（stored process）

## 创建存储过程

存储过程由以下部分组成：

1. 存储过程名称：唯一标识存储过程的名称。

2. 参数列表：定义存储过程需要的输入或输出参数。

3. SQL语句：实际执行的SQL语句集合。

4. 控制流程：定义存储过程如何处理条件、循环和异常等情况。

以下是创建和使用MySQL存储过程的基本步骤：

1. 使用CREATE PROCEDURE语句创建存储过程：

```mysql
CREATE PROCEDURE procedure_name (IN input_parameter1 data_type1, IN input_parameter2 data_type2, OUT output_parameter data_type)
BEGIN
  -- SQL statements here
END;
```

其中，procedure_name为存储过程名称，input_parameter和output_parameter是存储过程的输入和输出参数，data_type指定了参数的数据类型。在BEGIN和END之间是定义的存储过程的SQL语句。

在MySQL中，创建存储过程需要使用DELIMITER语句指定分隔符。默认情况下，MySQL使用分号（;）作为SQL语句和命令的分隔符。如果没有修改分隔符，MySQL会第一个分号视为存储过程定义语句的结束符，会导致错误。

可以使用DELIMITER语句定义新的分隔符，例如“$$”，然后在存储过程结束时再将分隔符重置为默认值：

```mysql
DELIMITER $$
CREATE PROCEDURE my_procedure()
BEGIN
  -- SQL statements here
END $$
DELIMITER ;
```

注意，在上面的示例中，我们将分隔符从默认的分号设置为两个美元符号“$$”。当存储过程定义完毕后，我们需要将分隔符重置成分号以便后续操作。

## 调用存储过程

```mysql
CALL procedure_name(input_value1, input_value2, @output_value);
```

其中，input_value1和input_value2是存储过程的传入参数值，@output_value是存储过程的输出参数。您可以用SELECT语句检索@output_value的值。

需要注意的是，MySQL存储过程支持条件、循环和异常处理等复杂的控制流程结构。存储过程可以将这些结构与SQL语句组合在一起，以完成特定的任务或操作。

 存储过程的输出参数有个@的原因： 

在MySQL中，存储过程参数分为输入参数和输出参数。与输入参数不同，输出参数必须使用@前缀来声明。

这是因为MySQL中的@符号表示用户定义变量（User-Defined Variables），可以在多个SQL语句之间传递值。当在存储过程中声明一个输出参数时，实际上是在创建一个用户定义变量，以便将结果从存储过程传递出去。

例如，下面是一个简单的存储过程示例，它将两个整数相加，并将结果存储在输出参数中：

```mysql
CREATE PROCEDURE add_numbers(IN num1 INT, IN num2 INT, OUT result INT)
BEGIN
  SET result = num1 + num2;
END;
```

在执行该存储过程时，需要声明一个用户定义变量，并在调用存储过程时将其传递给输出参数：

```mysql
SET @output_value = 0;
CALL add_numbers(10, 20, @output_value);
SELECT @output_value;
```

在上面的代码中，我们首先使用SET语句创建了一个名为@output_value的用户定义变量，并将其初始化为0。然后，我们调用了add_numbers存储过程，并将10和20作为输入参数传递给它。最后，我们使用SELECT语句检索输出参数的值。注意，我们使用了@前缀来引用输出参数。

需要注意的是，在MySQL中，用户定义变量的作用域仅限于当前会话（Session）。这意味着在存储过程内部定义的用户定义变量不能在存储过程外部使用，反之亦然。

## 显示存储过程

要在MySQL中查看存储过程的定义，可以使用SHOW CREATE PROCEDURE语句。该语句将显示与存储过程相关的详细信息，包括存储过程名称、参数列表和SQL语句。

以下是一个示例存储过程，它返回员工的姓名和薪水：

```mysql
CREATE PROCEDURE get_employee(IN employee_id INT, OUT employee_name VARCHAR(50), OUT employee_salary DECIMAL(10,2))
BEGIN
  SELECT name, salary INTO employee_name, employee_salary FROM employees WHERE id = employee_id;
END;
```

要查看该存储过程的定义，请执行以下语句：

```mysql
SHOW CREATE PROCEDURE get_employee;
```

执行结果类似于下面的内容：

```mysql
Procedure: get_employee
Create Procedure: CREATE PROCEDURE `get_employee`(IN employee_id INT, OUT employee_name VARCHAR(50), OUT employee_salary DECIMAL(10,2))
BEGIN
  SELECT name, salary INTO employee_name, employee_salary FROM employees WHERE id = employee_id;
END
```

其中，第一行显示了存储过程的名称（Procedure: get_employee），而第二行则显示了完整的CREATE PROCEDURE语句，包括存储过程的定义。

需要注意的是，如果您没有对存储过程具有足够的权限，SHOW CREATE PROCEDURE语句可能会返回“Access denied”错误。在这种情况下，您需要确保已经授予了足够的权限，并且使用正确的MySQL用户来执行该语句。

## 删除存储过程

在MySQL中删除存储过程非常简单。您可以使用DROP PROCEDURE语句来删除一个或多个存储过程。

以下是删除名为get_employee的存储过程的示例：

```mysql
DROP PROCEDURE IF EXISTS get_employee;
```

在这个示例中，我们使用了DROP PROCEDURE语句来删除名称为get_employee的存储过程。如果该存储过程不存在，它将不会产生任何影响。如果存在同名的存储过程，则该命令将永久删除该存储过程，而无法撤消操作。

IF EXISTS的作用参见前面章节的删除数据库。

# 存储函数

> 存储函数（stored function）的操作方法和存储过程基本相同，与存储过程的唯一不同是，存储函数在执行后只会返回一个值，主要用于返回单个值而不是执行操作。
>
> 存储函数可以接受零个或多个输入参数，并返回一个标量值（例如整数、字符串、日期等）
>
> 如果需要在SQL查询中计算一些复杂的表达式或逻辑，则可以使用存储函数来简化查询语句
>
> MySQL有许多函数，但使用存储函数可以创建自定义的函数，所以存储函数也称为用户定义函数

## 启用存储函数日志

当log_bin_trust_function_creators为0时，用户不能创建或更新存储函数。

在MySQL中，log_bin_trust_function_creators是一个系统变量，它用于控制是否记录创建存储函数（CREATE FUNCTION语句）的操作到二进制日志中。这个变量的默认值为0（关闭，OFF），这意味着MySQL不会将CREATE FUNCTION语句记录到二进制日志中。把log_bin_trust_function_creators设置为1，将允许用户创建和修改存储函数，并且这些操作也将被记录到二进制日志中。

需要注意的是，即使log_bin_trust_function_creators被设置为1，用户也需要适当的权限才能够创建、修改或删除存储函数。这包括CREATE ROUTINE、ALTER ROUTINE和DROP ROUTINE等权限。如果用户没有这些权限，则无法执行相关操作。

要启用log_bin_trust_function_creators，可以采用以下方法：

1. 使用SET

   ```mysql
   SET GLOBAL log_bin_trust_function_creators = 1;
   ```

   

2. 修改my.cnf

   ```ini
   [mysqld]
   log_bin_trust_function_creators = 1
   ```

要查看log_bin_trust_function_creators，可以采用以下方法：

```mysql
SHOW VARIABLES LIKE 'log_bin_trust_function_creators';
```

## 创建存储函数

以下是一个计算两个整数之和的示例存储函数：

```mysql
CREATE FUNCTION add_numbers(num1 INT, num2 INT) RETURNS INT
BEGIN
  DECLARE result INT; -- 语法是 DECLARE 变量名 数据类型
  SET result = num1 + num2; 
  -- 或者使用SELECT INTO，示例：SELECT AVG(sales) INTO result FROM tb;
  RETURN result;
END;
```

在这个示例中，我们定义了一个名为add_numbers的存储函数，它接受两个整数作为输入参数，并返回它们的和。在存储函数内部，我们使用DECLARE语句声明了一个名为result的局部变量，并将num1和num2的和赋值给它。最后，我们使用RETURN语句返回result的值。

要调用该存储函数，请使用SELECT语句进行查询：

```mysql
SELECT add_numbers(10, 20);
```

在此查询中，我们调用了add_numbers函数，并传递了10和20作为两个输入参数。该函数将返回它们的和，即30。

需要注意的是，在MySQL中，存储函数应该具有确定性，也就是说，对于相同的输入参数，它应该始终返回相同的结果。此外，存储函数还应该具有不产生副作用的性质，也就是说，它不应该修改数据库中的数据或执行任何有意义的操作。如果存储函数违反了这些规则，可能会导致不可预测的结果或安全问题。

## 显示存储函数

### 显示数据库中所有的存储函数

你可以使用MySQL的SHOW FUNCTION STATUS语句来显示所有定义在数据库中的存储函数。

具体来说，该语句的语法如下：
```mysql
SHOW FUNCTION STATUS;
```
执行该语句将返回包含以下列的结果集：

1. Db：与存储函数相关联的数据库名称。
2. Name：存储函数的名称。
3. Type：函数类型（标量函数 [SCALAR FUNCTION]、聚合函数 [AGGREGATE FUNCTION]、窗口函数 [WINDOW FUNCTION]）。
4. Definer：函数创建者。
5. Modified：最近一次修改时间。
6. Created：创建时间。
7. Security_type：安全模式。

此外，还有其他可选的列，例如Comment和character_set_client。

如果你想查看特定表的存储函数，你需要在SHOW FUNCTION STATUS语句后面添加LIKE子句来指定要查询的模式，例如：

```mysql
SHOW FUNCTION STATUS LIKE 'myfunction%';
```

上面的示例将返回名称以“myfunction”开头的所有存储函数。

### 显示指定存储函数

```mysql
SHOW CREATE FUNCTION function_name;
```

## 删除存储函数

删除存储函数的语法格式是：

```mysql
DROP FUNCTION 存储函数名;
```

# 触发器

> MySQL触发器（trigger）是一种数据库对象，它允许你在特定的表上定义自动执行的操作，当满足触发器定义的条件时，该操作将自动触发。
>
> 常见的使用场景包括：
>
> - 在记录插入/更新/删除操作时自动生成日志记录。
> - 在记录插入/更新/删除操作时同步到其他相关的表。
> - 在记录插入/更新时进行验证或格式化等操作。
>
> 需要注意的是，MySQL触发器在处理大量数据时可能会显著减慢数据库性能，因此应谨慎使用。

## 创建触发器

一个MySQL触发器包含以下几个部分：

1. 触发器名称：用于标识触发器的名称。
2. 关联表名称：需要关联到触发器的表名。
3. 事件类型：可以是INSERT、UPDATE或DELETE，表示在关联表中执行的操作类型。
4. 触发时间：可以是BEFORE或AFTER，指定何时执行触发操作。
5. 条件：指定触发操作的条件，可以使SQL语句中的WHERE子句。
6. 触发操作：在满足触发器条件时要执行的操作，可以是一条或多条SQL语句。

下面是一个创建MySQL触发器的示例：

```mysql
CREATE TRIGGER my_trigger
AFTER INSERT ON my_table
FOR EACH ROW
BEGIN
    -- 触发操作
END;
```

在上述示例中，创建了一个名为my_trigger的触发器，设置为在my_table表中进行插入操作之后自动调用。FOR EACH ROW关键字表示该操作针对每个插入的行都会执行一次。在BEGIN和END之间的代码块则是要执行的SQL语句操作。

## OLD、NEW关键字

在MySQL触发器中，可以使用`OLD`和`NEW`关键字来获取一个操作（如 INSERT、UPDATE 或 DELETE）的旧值和新值。

- OLD.列名

  OLD.列名是对表处理前的列值

  INSERT事件类型没有OLD.列名

- NEW.列名

  NEW.列名是对表处理后的列值

  DELETE事件类型没有NEW.列名

示例：

```mysql
CREATE TRIGGER delete_trigger
BEFORE DELETE ON employee
FOR EACH ROW
BEGIN
    INSERT INTO log_table (id_log_table, emp_id_log_table) VALUES (OLD.id, OLD.emp_id);
END;
```

在上述示例中，`OLD`关键字被用于访问将要被删除信息的行（旧行）。在此案例中，每次将要删除一行之前，都会将此行的ID和列1插入一个名为log_table的表中。

## 显示触发器

如果你想要显示MySQL数据库中的触发器，可以使用以下命令：

```mysql
SHOW TRIGGERS;
```

当你执行上述命令时，将返回包含所有已创建触发器的结果集。该结果集包括每个触发器的名称、关联表、事件类型、触发时间和触发语句。

如果你只想检索指定表的触发器，则可以使用以下语法：

```mysql
SHOW TRIGGERS FROM your_database_name LIKE 'your_table_name';
```

需要将“your_database_name”替换为你要查询的数据库名称，“your_table_name”替换为你要查询的表名称。这条命令将返回满足特定条件的结果集。

## 删除触发器

要删除一个已经存在的MySQL触发器，可以使用以下语法：

```mysql
DROP TRIGGER [IF EXISTS] trigger_name;
```

需要将“trigger_name”替换为你要删除的触发器的名称。可选的参数IF EXISTS表示如果该触发器不存在，也不会抛出错误并且继续执行下一个命令。

# 存储引擎

在MySQL中，存储引擎是一种用来处理数据的组件，它决定了如何存储、读取和更新数据。MySQL支持多种不同的存储引擎。

常见的MySQL存储引擎包括：

1. InnoDB引擎：InnoDB是MySQL默认的存储引擎，支持事务和行级锁，它可以提供更好的并发性能和可靠性，适合于处理大量更新操作的数据表。 

2. MyISAM引擎：MyISAM是MySQL早期的一种存储引擎，不支持事务，不支持外键，但支持全文索引和压缩等功能，适合于查询频繁、更新不频繁的数据表。

3. Memory引擎：Memory存储引擎将表数据存储在内存中，支持非常快速的读写操作，但数据会在MySQL重新启动或重启后丢失，适合于缓存和临时表等应用。

4. Archive引擎：Archive存储引擎只支持查询和插入操作，不支持更新和删除操作，可以对历史数据进行归档和压缩，节省磁盘空间。

除了上述存储引擎之外，还有其他一些存储引擎，如CSV、Blackhole、Federated等，在实际应用中根据业务需求选择不同的存储引擎进行优化。需要注意的是，不同的存储引擎在性能和功能方面有很大差异，开发人员应该根据具体情况进行选择和调整。

## 显示存储引擎

可以通过显示表的创建信息命令来查看使用的存储引擎

```mysql
SHOW CREATE TABLE employee; 
```

输出结果示例如下：

```mysql
CREATE TABLE `employee` (
  `id` int NOT NULL,
  `emp_id` char(10) DEFAULT NULL,
  `emp_name` varchar(10) DEFAULT NULL,
  `manager_id` char(10) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

## 修改存储引擎

如果需要修改已有表的存储引擎，可以使用`ALTER TABLE`语句来实现，例如：

```mysql
ALTER TABLE table_name ENGINE=MyISAM;
```

使用上述语句将会将名为`table_name`的表的存储引擎修改为`MyISAM`。

# 事务

> 将多个操作作为单个逻辑工作单元处理的功能称为事务（transaction）
>
> 将事务开始的处理结果反映到数据库的操作称为提交（commit）
>
> 不反映到数据库中而是保持恢复为原来状态的操作称为回滚（rollback）

## 开启事务

如果多个操作需要一起进行，可以将其放在一个事务中，以保证所有操作都要么全部成功，要么全部失败。

在MySQL中，使用以下语句来开始一个事务：

```mysql
START TRANSACTION;
-- 或者 BEGIN;
-- 或者 BEGIN WORK;
```

`BEGIN`和`BEGIN WORK`命令也可以用于开启一个新的事务。它们与`START TRANSACTION`命令作用相同，都可以开启一个新的事务，让后续的SQL语句在这个事务内执行。

## 提交事务

如果所有的操作都成功了，则使用以下语句来提交事务

```mysql
COMMIT;
```

在MySQL中，有一个自动提交（autocommit）功能，默认情况下开启。当执行单个SQL语句时，会自动将该语句的修改内容提交到数据库，即将其作为一个事务进行处理。

如果需要关闭自动提交功能，则可以使用以下命令：

```mysql
SET autocommit = 0;
```

这样，在执行多个SQL语句时，就需要手动调用BEGIN或START TRANSACTION命令开始一个新的事务，再通过COMMIT或ROLLBACK命令结束事务。例如：

```mysql
SET autocommit = 0;

-- 开始一个新的事务
BEGIN;

-- 执行一些SQL语句
INSERT INTO table1 (column1, column2) VALUES (value1, value2);

-- 判断是否发生错误
IF some_error_occured THEN
    -- 回滚事务
    ROLLBACK;
ELSE
    -- 提交事务
    COMMIT;
END IF;
```

以上代码首先关闭了自动提交功能，然后使用BEGIN命令开始了一个新的事务，在其中执行了一些SQL语句，并根据情况选择了提交或回滚事务。

## 回滚事务

需要注意的是，在MySQL中，`BEGIN`、`BEGIN WORK`和`START TRANSACTION`命令也可以配合使用，例如：

```mysql
BEGIN WORK;
-- 或者 BEGIN;
-- 或者 START TRANSACTION;

-- 执行一些SQL操作

SAVEPOINT sp1;

-- 执行一些SQL操作

ROLLBACK TO SAVEPOINT sp1;

-- 执行一些SQL操作

COMMIT;
```

以上示例中，除了开启环节的不同之外，还涉及到了MySQL事务中的保存点（SAVEPOINT）概念。保存点是指在事务内定义的一个标记，用于标识事务内某个时刻的状态。当事务遇到错误并进行回滚时，可以将事务恢复到保存点所标识的状态。

# 调优

调优可以提高数据库的性能和吞吐量。以下是一些MySQL调优的方法：

1. 优化查询语句

- 使用合适的索引：对于经常用于检索的列，创建索引可大幅提升查询效率；
- 避免使用SELECT *：只选取必要的列可以减少数据传输和磁盘I/O；
- 使用EXPLAIN命令查看查询执行计划：通过观察查询计划，可以了解到查询过程中哪些步骤需要优化。

2. 调整服务器参数

- 修改MySQL缓冲区大小：将innodb_buffer_pool_size设置为合理的大小，以便在缓存中保留更多的数据；
- 调整处理器缓存和线程池：根据服务器的硬件规格和应用程序类型，适当增加线程池大小和处理器缓存大小，以提高并发处理能力；
- 修改文件系统缓存大小：根据服务器的硬件和操作系统，可以修改磁盘缓存大小；

3. 优化表结构设计

- 使用恰当的数据类型：为每个列选择最小、最合适的数据类型可以减少磁盘空间和内存开销，提高查询速度；
- 避免使用太多的JOIN：JOIN操作需要较多的CPU和内存资源，应尽可能减少其使用；
- 分解大的表：当一个表中包含大量数据时，可以考虑将其分解为多个较小的表，以提高查询和更新效率。

4. 监控数据库性能

- 使用SHOW STATUS或SHOW GLOBAL STATUS命令查看MySQL性能统计信息；
- 采用监控工具进行实时监控，如Nagios、Zabbix等。

总而言之，在调优MySQL时需要综合考虑硬件、操作系统、数据库参数等各方面因素，对于常见的优化点进行一一梳理和测试，以找到最佳配置参数。

# B树、B+树

B树和B+树都是常见的数据库索引结构，用于优化对数据库的查询操作。它们有些类似，但又有不同。

1. B树

B树（B-tree）是一种自平衡树形索引结构，通常用于磁盘或其他直接存取外存储设备的文件系统。B树允许在对数时间内进行搜索、插入和删除等操作。B树的特点：

- 每个节点可以包含多个键值和链接；
- 所有叶子节点都位于同一层级，并且不含信息，只包含指向数据存储位置的指针；
- 非叶子节点的关键字作为索引；

2. B+树

B+树（B-plus tree）也是一种自平衡树形结构，其基本原理与B树相似，但在实现细节上有所不同，主要用于数据库和操作系统中。B+树的特点：

- 所有叶子节点包含全部的关键字和记录指针；
- 各叶子节点通过指针连接成一个有序链表；
- 内部节点不包含具体的数据，只包含索引信息；
- 相较于B树而言，B+树更适用于范围查询。

比较：

B+树优于B树的地方：

- 对于查找和范围查找，B+树只需要遍历叶子节点一次即可完成操作，而B树需要在内部节点和叶子节点都进行查找，因此B+树查询效率更高；
- 非叶子节点仅存储键值信息，可以容纳更多的索引项，从而降低树的高度，提高查询速度；
- B+树所有叶子节点形成一个双向链表，可以方便地支持区间查找和遍历。

B树优于B+树的地方：

- B树对于随机读取和插入数据有较好的性能，同时也不易受到磁盘块大小等参数的影响。B+树相对而言只适用于顺序访问和插入数据的情况；
- B树的非叶子节点和叶子节点的处理方式相同，实现较为简单，B+树则需要特殊的处理。

总体而言，B+树的广泛应用是由于其范围查询的效率极高，并且其结构使得查询很容易地通过迭代来实现。

# 备份和恢复数据库

MySQL有多种备份数据库的方式，包括物理备份和逻辑备份，在备份时，需要根据实际情况选择最合适的备份方式，并保证备份数据的一致性、完整性和安全性。

1. 物理备份

物理备份是指直接备份MySQL服务器中数据文件的一种方式。这种备份方式直接将数据文件复制到指定的备份目录下，并保持与原始数据文件的完全一致，因此恢复时也很快速。

物理备份包括两种主要类型：

- 冷备份：停止MySQL服务后备份数据文件，优点是备份数据的一致性好，缺点是在备份期间无法进行数据库操作。
- 热备份：不停止MySQL服务进行备份，优点是可以在备份时继续对数据库进行操作，缺点是备份数据可能会因为正在执行的事务而不完整或不一致。

物理备份的命令可以使用Linux中的cp、rsync等命令，例如：

```shell
$ cp -a /var/lib/mysql /backup/mysql_backup
```

2. 逻辑备份

逻辑备份是指通过SQL语句来生成备份文件，备份数据以可读性较好的文本格式保存，因此备份数据相对于物理备份较大，但可以进行较为精细的筛选和处理。

逻辑备份包括以下主要类型：

- mysqldump：可以备份指定的数据库或表，甚至可以备份数据库中的指定数据，生成.sql格式的备份文件。
- mydumper：适用于大型数据库，生成多个文件来备份数据。
- mysqlpump：在MySQL8.0及以上版本中提供了mysqlpump命令，该命令比mysqldump更快，也支持多线程和压缩。

例如，使用mysqldump备份一个名为mydatabase的数据库：

```shell
$ mysqldump -u root -p mydatabase > mydatabase_backup.sql
```

在备份时，需要注意一些重要的问题：

1. 备份文件应当保存在安全的位置，并进行良好的加密措施。
2. 常规的备份操作应当建立合理的时间间隔。
3. 对于生产环境的数据库，建议还使用主从复制等业务高可用方案来增加数据可靠性。

恢复MySQL备份文件则相对较简单，可以使用以下命令进行：

```shell
$ mysql -u root -p mydatabase < mydatabase_backup.sql
```

其中，mydatabase是需要恢复的数据库的名称，mydatabase_backup.sql是备份文件的路径。执行后，MySQL会将备份文件中的数据导入到MySQL服务器中，并重新生成所有的索引和约束条件，以保证数据的正确性和完整性。

在MySQL中，如果备份文件中的数据字符编码与目标数据库不一致，恢复数据时可能会出现乱码等问题。为了避免这种情况，可以通过指定字符集参数来将备份文件中的数据以正确的字符编码导入到目标数据库中。

以下是根据备份文件的字符集来重新设置字符集的命令示例：

```shell
$ mysql -u root -p mydatabase --default-character-set=utf8 < backup.sql
```

其中，mydatabase是需要恢复的数据库的名称，backup.sql是备份文件的路径。--default-character-set选项用于指定字符集，可以根据实际情况设置编码类型和字符集。如果备份文件的字符编码为GBK，则可以将上述命令修改如下：

```shell
$ mysql -u root -p mydatabase --default-character-set=gbk < backup.sql
```

在MySQL 5.5及以上版本中，默认的字符集为utf8mb4，因此如果备份文件中使用的是utf8或gbk等字符集，也需要显式地指定字符集参数。

另外，在备份时也应当考虑到字符编码的问题，建议在备份时同时备份字符集相关的信息。例如，在进行逻辑备份时，可以添加--set-charset选项来确保备份数据使用与数据库相同的字符集。例如：

```shell
$ mysqldump -u root -p mydatabase --set-charset > mydatabase_backup.sql
```

这样，在恢复数据时就无需再指定字符集，MySQL会自动使用与备份时相同的字符集导入数据。

# 查询

MySQL数据查询的主要过程：

1. 查询缓存

   MySQL会缓存查询结果，如果在缓存期间发生了数据更改，则缓存失效，会被删除。如果数据没有更改，则对于完全一致且不存在某些会改变查询含义的函数如NOW()，就会直接返回缓存中的内容。

   但是维护缓存是需要开销的，包括查询缓存、添加缓存、删除缓存、维护缓存内存区域。所以MySQL从5.7.20开始不推荐使用MySQL缓存功能，在MySQL8.0中直接将其删除了。

2. 语法解析

   属于编译过程，涉及词法分析、语法分析、语义分析等。

3. 查询优化

   

   

# Reference

- 西泽梦路. MySQL基础教程. 北京: 人民邮电出版社, 2020.1.

索引不适用的条件
索引列上有函数，不满足最左前缀，使用了不等号，使用了范围查询等等
索引的分类
B-Tree索引， Hash索引，全文索引，单值索引、唯一索引、复合索引
聚簇索引、非聚簇索引等等，
