# No.37 MySQL：自增ID归1

## 导读

> 自增ID不满足需求时可以改变。

### 1、清空所有数据，自增字段恢复从1开始计数

```sql
truncate table 表名;
```

### 2、自增字段恢复从1开始计数

```sql
alter table 表名 AUTO_INCREMENT=1;
```

### 3、清空具有外键约束的表时报ERROR 1701(42000)的解决方法

```sql
# 关闭外键约束
SET foreign_key_checks=0;

# 开启外键约束
SET foreign_key_checks=1;
```
