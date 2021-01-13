# No.53 Flask：常见问题汇总

## 导读

> 整合常见的Flask报错及解决方法。

### 1、连接Mysql时出现 Warning 1366

- 解决方法

```python
import mysql.connector

app.config["SQLALCHEMY_DATABASE_URI"] = "mysql+mysqlconnector://账号:密码@localhost/appname"

```
