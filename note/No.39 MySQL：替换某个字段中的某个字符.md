# No.39 MySQL：替换某个字段中的某个字符

## 导读

> MySQL实用小技巧。

```sql
number             addr
01             四川省成都市XXXXXX街道05号
02             四川省成都市XXXXXX街道07号
03             四川省成都市XXXXXX街道09号
04             四川省成都市XXXXXX街道04号
```

需求：addr字段里面的所有的值，都要把成都市改为天府市。

```sql
number             addr
01             四川省天府市XXXXXX街道05号
02             四川省天府市XXXXXX街道07号
03             四川省天府市XXXXXX街道09号
04             四川省天府市XXXXXX街道04号
```

解决方法：

sql语句：

```sql
update 表名 set 字段名=REPLACE (字段名,'原来的值','要修改的值');
```

当然，也可以添加条件：

```sql
update user_item set addr=REPLACE (addr,'成都','天府') where time<'2013-11--5';
```
