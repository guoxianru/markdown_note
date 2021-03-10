# No.76 Windows：用dism命令安装NET FRAMEWORK3.5

## 导读

> 在Windows10中运行一些软件程序的时候，可能会因为缺少.NET FRAMEWORK3.5环境无法运行，使用控制面板安装太慢，直接使用dism命令安装效率更高。

- 准备一个Windows10原版镜像，找到镜像\sources\sxs\文件夹，解压文件夹里包含‘netfx3’的文件。

- 以管理员身份运行Windows PowerShell。

- 输入命令。

```shell
dism.exe /online /add-package /packagepath:D:\microsoft-windows-netfx3-ondemand-package~31bf3856ad364e35~amd64~~.cab

```

- 重启系统即可。
