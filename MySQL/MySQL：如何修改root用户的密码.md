# MySQL：如何修改root用户的密码

## 导读

> 再也不用担心忘记密码了。

### 方法1：用SET PASSWORD命令

```sql
mysql> set password for 用户名@localhost = password('新密码');
# 举例
mysql> set password for root@localhost = password('123');
```

### 方法2：用mysqladmin

```sql
mysql> mysqladmin -u用户名 -p旧密码 password 新密码;
# 举例
mysql> mysqladmin -uroot -p123456 password 123;
```

### 方法3：用UPDATE直接编辑user表

```sql
mysql> use mysql;
mysql> update user set password=password('123') where user='root' and host='localhost';
mysql> flush privileges;
```

### 方法4：忘记root密码的时候

以windows为例：

1. 关闭正在运行的MySQL服务。

2. 打开DOS窗口，转到mysql\bin目录。

3. 输入mysqld --skip-grant-tables回车。--skip-grant-tables的意思是启动MySQL服务的时候跳过权限表认证。

4. 再开一个DOS窗口（因为刚才那个DOS窗口已经不能动了），转到mysql\bin目录。

5. 输入mysql回车，如果成功，将出现MySQL提示符 >

6. 连接权限数据库：use mysql;

7. 改密码：update user set password=password("123") where user="root";

8. 刷新权限（必须步骤）：flush privileges;

9. 退出：quit

10. 注销系统，再进入，使用用户名root和刚才设置的新密码123登录。
