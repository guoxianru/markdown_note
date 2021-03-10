# No.112 Python：使用 black 控制代码质量

## 导读

> Black，号称不妥协的代码格式化工具，它检测到不符合规范的代码风格直接就帮你全部格式化好，根本不需要你确定，直接替你做好决定。从而节省关注代码规范的时间和精力，专心编程。

### 1、Black安装

- Windows：

```shell
pip install black
```

black.exe 的安装位置在Python目录下Scripts\black.exe，需添加环境变量 PATH

- Linux：

```shell
sudo pip install black
```

black 的安装位置在 /usr/bin/black

### 2、配置 PyCharm

File > Settings> Tools > External Tools，点击 + 号添加

```shell
# Name(插件名称)
Black

# Description(插件描述)
Black

# Program(指向 Black 的安装目录)
C:\Users\Administrator\Envs\python36_tool\Scripts\black.exe

# Arguments(选择 Black 输出信息显示格式和要 disable 的项目)
$FilePath$

# Working directory(指定 Black 的工作目录)
C:\Users\Administrator\Envs\python36_tool\Scripts
```

### 3、使用 Black 评估代码质量

当写完一个脚本后，直接右键单击，选择 External Tools > Black，运行后可得到当前代码质量，和改进建议。
