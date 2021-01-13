# No.82 Python：使用 Pylint 控制代码质量

## 导读

> Pylint 是一个 Python 代码分析工具，它分析 Python 代码中的错误，查找不符合代码风格标准（Pylint 默认使用的代码风格是 PEP 8）和有潜在问题的代码。

### 1、Pylint安装

- Windows：

```shell
pip install pylint
```

pylint.exe 的安装位置在Python目录下Scripts\pylint.exe，需添加环境变量 PATH

- Linux：

```shell
sudo pip install pylint
```

pylint 的安装位置在 /usr/bin/pylint

### 2、配置 PyCharm

File > Settings> Tools > External Tools，点击 + 号添加

```shell
# Name(插件名称)
Pylint

# Description(插件描述)
Pylint

# Program(指向 Pylint 的安装目录)
C:\Users\Administrator\Envs\python36_tool\Scripts\pylint.exe

# Arguments(选择 Pylint 输出信息显示格式和要 disable 的项目)
-rn --msg-template="{abspath}:{line}: [{msg_id}({symbol}), {obj}] {msg}" $FilePath$

# Working directory(指定 Pylint 的工作目录)
C:\Users\Administrator\Envs\python36_spider\Scripts
```

### 3、使用 Pylint 评估代码质量

当写完一个脚本后，直接右键单击，选择 External Tools > Pylint，运行后可得到当前代码质量，和改进建议。
