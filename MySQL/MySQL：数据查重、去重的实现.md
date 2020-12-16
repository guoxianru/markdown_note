# MySQL：数据查重、去重的实现

## 导读

> 使用此命令时要考虑性能问题。

有一个表user，字段分别有id、nick_name、password、email、phone。

### 一、单字段（nick_name）

```sql
# 查出所有有重复记录的所有记录
select * from user where nick_name in (select nick_name from user group by nick_name having count(nick_name)>1);

# 查出有重复记录的各个记录组中id最大的记录
select * from user where id in (select max(id) from user group by nick_name having count(nick_name)>1);

# 查出多余的记录，不查出id最小的记录
select * from user where nick_name in (select nick_name from user group by nick_name having count(nick_name)>1) and id not in (select min(id) from user group by nick_name having count(nick_name)>1);

# 删除多余的重复记录，只保留id最小的记录
delete from user where nick_name in (select nick_name from (select nick_name from user group by nick_name having count(nick_name)>1) as tmp1) and id not in (select id from (select min(id) from user group by nick_name having count(nick_name)>1) as tmp2);
```

### 二、多字段（nick_name,password）

```sql
# 查出所有有重复记录的记录
select * from user where (nick_name,password) in (select nick_name,password from user group by nick_name,password where having count(nick_name)>1);

# 查出有重复记录的各个记录组中id最大的记录
select * from user where id in (select max(id) from user group by nick_name,password where having count(nick_name)>1);

# 查出各个重复记录组中多余的记录数据，不查出id最小的一条
select * from user where (nick_name,password) in (select nick_name,password from user group by nick_name,password having count(nick_name)>1) and id not in (select min(id) from user group by nick_name,password having count(nick_name)>1);

# 删除多余的重复记录，只保留id最小的记录
delete from user where (nick_name,password) in (select nick_name,password from (select nick_name,password from user group by nick_name,password having count(nick_name)>1) as tmp1) and id not in (select id from (select min(id) id from user group by nick_name,password having count(nick_name)>1) as tmp2);
```
