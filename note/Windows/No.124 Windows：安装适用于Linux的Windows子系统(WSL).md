# No.124 Windows：安装适用于Linux的Windows子系统(WSL)

## 导读

> 适用于 Linux 的 Windows 子系统可让开发人员按原样运行 GNU/Linux 环境 - 包括大多数命令行工具、实用工具和应用程序 - 且不会产生传统虚拟机或双启动设置开销。

### 1、安装WSL

- 启用适用于 Linux 的 Windows 子系统

```shell
# 以管理员身份打开 PowerShell 并运行
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

- 检查系统版本

```shell
# Windows + R
winver
```

- 启用虚拟机功能

```shell
# 以管理员身份打开 PowerShell 并运行
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

- 下载 Linux 内核更新包

Linux 内核更新包 [微软](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

- 将 WSL 2 设置为默认版本

```shell
# 以管理员身份打开 PowerShell 并运行
wsl --set-default-version 2
```

- 安装所选的 Linux 分发

打开 Microsoft Store，并选择你偏好的 Linux 分发版。

### 2、使用WSL安装Centos

可用于WSL的CentOS镜像 [GitHub](https://github.com/yuk7/CentWSL)

- 解压WSL CentOS 7.x

- 右键点击并以管理员身份运行CentOS.exe并安装

- 在WSL上卸载CentOS

如果您因为某种原因不想使用了，可以使用下面命令在WSL上卸载CentOS：

```shell
./CentOS.exe clean
```
