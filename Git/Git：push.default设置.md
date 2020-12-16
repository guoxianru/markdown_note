# Git：push.default设置

## 导读

> Git初装后提交仓库可能遇到的问题。

### 1、在进行一次仓库的提交时，我遇到了这个警告，警告如下

```shell
warning: push.default未设置
它的默认值将会在Git 2.0由'matching'修改为 'simple'
若要不再显示本信息并在其默认值改变后维持当前使用习惯,进行如下设置
git config --global push.default matching
若要不再显示本信息并从现在开始采用新的使用习惯,设置
git config --global push.default simple
参见 'git help config' 并查找 'push.default' 以获取更多信息
'simple' 模式由 Git 1.7.11 版本引入
如果您有时要使用老版本的 Git，为保持兼容，请用 'current' 代替 'simple' 模式
```

### 2、当然首先我要先说明一下我电脑上git的版本，如下

```shell
git --version

git version 2.1
```

### 3、之后我查找了一些关于git push.default设置的知识

默认配置下，当使用git push命令而没有明确的指名本地分支和远程参考分支的情况下，会有如上的提示。

如果git push命令没有明确指定引用规格(refspec),也就是没有指定推送的源分支和目标分支，那么git会采用push.default定义的动作。不同的值适用于不同的工作流程模式。

### 4、显而易见，主要是因为之前没有进行设置引用规格才出现的这种问题，现在我把push.default的可用值与配置方法贴在下面

push.default可用的值如下：

1. nothing，不推送任何东西并有错误提示，除非明确指定分支引用规格。强制使用分支引用规格来避免可能潜在的错误。

2. current，推送当前分支到接收端名字相同的分支。

3. upstream，推送当前分支到上游@{upstream}。这个模式只适用于推送到与拉取数据相同的仓库，比如中央工作仓库流程模式。

4. simple，在中央仓库工作流程模式下，拒绝推送到上游与本地分支名字不同的分支。也就是只有本地分支名和上游分支名字一致才可以推送，就算是推送到不是拉取数据的远程仓库，只要名字相同也是可以的。在GIT 2.0中，simple将会是push.default的默认值。simple只会推送本地当前分支。

5. matching，推送本地仓库和远程仓库所有名字相同的分支。这是git当前版本的缺省值。

### 5、一般来说我们使用simple就可以进行正常的使用，如果严格一点儿可以用nothing

配置push.default的命令如下：

```shell
git config --global push.default simple
```
