# No.34 MySQL：内连接、外连接、左连接、右连接、全连接

## 导读

> 用两个表（a_table、b_table），关联字段a_table.a_id和b_table.b_id来演示一下MySQL的内连接、外连接（ 左(外)连接、右(外)连接、全(外)连接）。

- MySQL版本：Server version: 5.7.25 MySQL Community Server (GPL)

- 数据库表：a_table、b_table

- 主题：内连接、左连接（左外连接）、右连接（右外连接）、全连接（全外连接）

### 1、插入测试数据

```sql
CREATE TABLE `a_table` (
  `a_id` int(11) DEFAULT NULL,
  `a_name` varchar(10) DEFAULT NULL,
  `a_part` varchar(10) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `b_table` (
  `b_id` int(11) DEFAULT NULL,
  `b_name` varchar(10) DEFAULT NULL,
  `b_part` varchar(10) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### 2、内连接

关键字：inner join on

```sql
select * from a_table a inner join b_table b on a.a_id = b.b_id;
```

组合两个表中的记录，返回关联字段相符的记录。

### 3、左连接（左外连接）

关键字：left join on / left outer join on

```sql
select * from a_table a left join b_table b on a.a_id = b.b_id;
```

left join 是left outer join的简写，它的全称是左外连接，是外连接中的一种。

左(外)连接，左表(a_table)的记录将会全部表示出来，而右表(b_table)只会显示符合搜索条件的记录。右表记录不足的地方均为NULL。

### 4、右连接（右外连接）

关键字：right join on / right outer join on

```sql
select * from a_table a right join b_table b on a.a_id = b.b_id;
```

right join是right outer join的简写，它的全称是右外连接，是外连接中的一种。

与左(外)连接相反，右(外)连接，左表(a_table)只会显示符合搜索条件的记录，而右表(b_table)的记录将会全部表示出来。左表记录不足的地方均为NULL。

### 5、全连接（全外连接）

MySQL目前不支持此种方式，可以用其他方式替代解决。
