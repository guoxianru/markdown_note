# No.33 MySQL：创建视图

## 导读

> 创建视图是指在已经存在的MySQL数据库表上建立视图。

### 1、基本语法

可以使用 CREATE VIEW 语句来创建视图。

语法格式如下：

```sql
CREATE VIEW <视图名> AS <SELECT语句>;
```

语法说明如下。

<视图名>：指定视图的名称。该名称在数据库中必须是唯一的，不能与其他表或视图同名。

<SELECT语句>：指定创建视图的 SELECT 语句，可用于查询多个基础表或源视图。

对于创建视图中的 SELECT 语句的指定存在以下限制：

- 用户除了拥有 CREATE VIEW 权限外，还具有操作中涉及的基础表和其他视图的相关权限。

- SELECT 语句不能引用系统或用户变量。

- SELECT 语句不能包含 FROM 子句中的子查询。

- SELECT 语句不能引用预处理语句参数。

视图定义中引用的表或视图必须存在。但是，创建完视图后，可以删除定义引用的表或视图。可使用 CHECK
TABLE 语句检查视图定义是否存在这类问题。

视图定义中允许使用 ORDER BY 语句，但是若从特定视图进行选择，而该视图使用了自己的 ORDER BY
语句，则视图定义中的 ORDER BY 将被忽略。

视图定义中不能引用 TEMPORARY 表（临时表），不能创建 TEMPORARY 视图。

WITH CHECK OPTION 的意思是，修改视图时，检查插入的数据是否符合 WHERE 设置的条件。

### 2、创建基于单表的视图

MySQL 可以在单个数据表上创建视图。

查看 test_db 数据库中的 tb_students_info 表的数据，如下所示。

```sql
mysql> SELECT * FROM tb_students_info;

+----+--------+---------+------+------+--------+------------+
| id | name   | dept_id | age  | sex  | height | login_date |
+----+--------+---------+------+------+--------+------------+
|  1 | Dany   |       1 |   25 | F    |    160 | 2015-09-10 |
|  2 | Green  |       3 |   23 | F    |    158 | 2016-10-22 |
|  3 | Henry  |       2 |   23 | M    |    185 | 2015-05-31 |
|  4 | Jane   |       1 |   22 | F    |    162 | 2016-12-20 |
|  5 | Jim    |       1 |   24 | M    |    175 | 2016-01-15 |
|  6 | John   |       2 |   21 | M    |    172 | 2015-11-11 |
|  7 | Lily   |       6 |   22 | F    |    165 | 2016-02-26 |
|  8 | Susan  |       4 |   23 | F    |    170 | 2015-10-01 |
|  9 | Thomas |       3 |   22 | M    |    178 | 2016-06-07 |
| 10 | Tom    |       4 |   23 | M    |    165 | 2016-08-05 |
+----+--------+---------+------+------+--------+------------+
10 rows in set (0.00 sec)
```

【实例 1】在tb_students_info表上创建一个名为view_students_info的视图，输入的SQL语句和执行结果如下所示。

```sql
mysql> CREATE VIEW view_students_info AS SELECT * FROM tb_students_info;

Query OK, 0 rows affected (0.00 sec)

mysql> SELECT * FROM view_students_info;

+----+--------+---------+------+------+--------+------------+
| id | name   | dept_id | age  | sex  | height | login_date |
+----+--------+---------+------+------+--------+------------+
|  1 | Dany   |       1 |   25 | F    |    160 | 2015-09-10 |
|  2 | Green  |       3 |   23 | F    |    158 | 2016-10-22 |
|  3 | Henry  |       2 |   23 | M    |    185 | 2015-05-31 |
|  4 | Jane   |       1 |   22 | F    |    162 | 2016-12-20 |
|  5 | Jim    |       1 |   24 | M    |    175 | 2016-01-15 |
|  6 | John   |       2 |   21 | M    |    172 | 2015-11-11 |
|  7 | Lily   |       6 |   22 | F    |    165 | 2016-02-26 |
|  8 | Susan  |       4 |   23 | F    |    170 | 2015-10-01 |
|  9 | Thomas |       3 |   22 | M    |    178 | 2016-06-07 |
| 10 | Tom    |       4 |   23 | M    |    165 | 2016-08-05 |
+----+--------+---------+------+------+--------+------------+
10 rows in set (0.04 sec)
```

默认情况下，创建的视图和基本表的字段是一样的，也可以通过指定视图字段的名称来创建视图。

【实例 2】在tb_students_info表上创建一个名为v_students_info的视图，输入的SQL语句和执行结果如下所示。

```sql
mysql> CREATE VIEW v_students_info (s_id,s_name,d_id,s_age,s_sex,s_height,s_date) AS SELECT id,name,dept_id,age,sex,height,login_date FROM tb_students_info;

Query OK, 0 rows affected (0.06 sec)

mysql> SELECT * FROM v_students_info;

+------+--------+------+-------+-------+----------+------------+
| s_id | s_name | d_id | s_age | s_sex | s_height | s_date     |
+------+--------+------+-------+-------+----------+------------+
|    1 | Dany   |    1 |    24 | F     |      160 | 2015-09-10 |
|    2 | Green  |    3 |    23 | F     |      158 | 2016-10-22 |
|    3 | Henry  |    2 |    23 | M     |      185 | 2015-05-31 |
|    4 | Jane   |    1 |    22 | F     |      162 | 2016-12-20 |
|    5 | Jim    |    1 |    24 | M     |      175 | 2016-01-15 |
|    6 | John   |    2 |    21 | M     |      172 | 2015-11-11 |
|    7 | Lily   |    6 |    22 | F     |      165 | 2016-02-26 |
|    8 | Susan  |    4 |    23 | F     |      170 | 2015-10-01 |
|    9 | Thomas |    3 |    22 | M     |      178 | 2016-06-07 |
|   10 | Tom    |    4 |    23 | M     |      165 | 2016-08-05 |
+------+--------+------+-------+-------+----------+------------+
10 rows in set (0.01 sec)
```

可以看到，view_students_info和v_students_info两个视图中的字段名称不同，但是数据却相同。因此，在使用视图时，可能用户不需要了解基本表的结构，更接触不到实际表中的数据，从而保证了数据库的安全。

### 3、创建基于多表的视图

MySQL 中也可以在两个以上的表中创建视图，使用 CREATE VIEW 语句创建。

【实例 3】在表 tb_student_info 和表 tb_departments 上创建视图 v_students_info，输入的 SQL 语句和执行结果如下所示。

```sql
mysql> CREATE VIEW v_students_info (s_id,s_name,d_id,s_age,s_sex,s_height,s_date) AS SELECT id,name,dept_id,age,sex,height,login_date FROM tb_students_info;

Query OK, 0 rows affected (0.06 sec)

mysql> SELECT * FROM v_students_info;

+------+--------+------+-------+-------+----------+------------+
| s_id | s_name | d_id | s_age | s_sex | s_height | s_date     |
+------+--------+------+-------+-------+----------+------------+
|    1 | Dany   |    1 |    24 | F     |      160 | 2015-09-10 |
|    2 | Green  |    3 |    23 | F     |      158 | 2016-10-22 |
|    3 | Henry  |    2 |    23 | M     |      185 | 2015-05-31 |
|    4 | Jane   |    1 |    22 | F     |      162 | 2016-12-20 |
|    5 | Jim    |    1 |    24 | M     |      175 | 2016-01-15 |
|    6 | John   |    2 |    21 | M     |      172 | 2015-11-11 |
|    7 | Lily   |    6 |    22 | F     |      165 | 2016-02-26 |
|    8 | Susan  |    4 |    23 | F     |      170 | 2015-10-01 |
|    9 | Thomas |    3 |    22 | M     |      178 | 2016-06-07 |
|   10 | Tom    |    4 |    23 | M     |      165 | 2016-08-05 |
+------+--------+------+-------+-------+----------+------------+
10 rows in set (0.01 sec)
```

通过这个视图可以很好地保护基本表中的数据。视图中包含 s_id、s_name 和 dept_name，s_id 字段对应 tb_students_info 表中的 id 字段，s_name 字段对应 tb_students_info 表中的 name 字段，dept_name 字段对应 tb_departments 表中的 dept_name 字段。

### 4、查询视图

视图一经定义之后，就可以如同查询数据表一样，使用SELECT语句查询视图中的数据，语法和查询基础表的数据一样。

视图用于查询主要应用在以下几个方面：

- 使用视图重新格式化检索出的数据。

- 使用视图简化复杂的表连接。

- 使用视图过滤数据。

DESCRIBE 可以用来查看视图，语法如下：

```sql
DESCRIBE 视图名；
```

【实例 4】通过 DESCRIBE 语句查看视图 v_students_info 的定义，输入的SQL语句和执行结果如下所示。

```sql
mysql> DESCRIBE v_students_info;

+----------+---------------+------+-----+------------+-------+
| Field    | Type          | Null | Key | Default    | Extra |
+----------+---------------+------+-----+------------+-------+
| s_id     | int(11)       | NO   |     | 0          |       |
| s_name   | varchar(45)   | YES  |     | NULL       |       |
| d_id     | int(11)       | YES  |     | NULL       |       |
| s_age    | int(11)       | YES  |     | NULL       |       |
| s_sex    | enum('M','F') | YES  |     | NULL       |       |
| s_height | int(11)       | YES  |     | NULL       |       |
| s_date   | date          | YES  |     | 2016-10-22 |       |
+----------+---------------+------+-----+------------+-------+
7 rows in set (0.04 sec)
```

注意：DESCRIBE 一般情况下可以简写成 DESC，输入这个命令的执行结果和输入 DESCRIBE 是一样的。
