# Windows10：启用或禁用休眠、保留的存储

## 导读

> 本文介绍如何在运行Windows10的计算机上启用或禁用休眠，以及使用DISM命令启用或禁用保留的存储。

- 休眠

如果您禁止休眠，并且当混合睡眠设置打开时出现断电，您可能会丢失数据。禁用休眠时，混合睡眠将无法工作。

```shell
# 关闭
powercfg -h off
# 开启
powercfg -h on
```

- 保留的存储

在Windows10上，保留存储是一项功能，通过保留存储，将留出一些磁盘空间以供更新、应用程序、临时文件和系统缓存使用。目标是通过确保关键的操作系统功能始终可以访问磁盘空间来改善电脑的日常功能。

如果没有保留的存储空间，当用户用完了他的存储空间后，则Windows系统和应用程序运行将变得不可靠。通过保留存储功能，当电脑的可用空间用完时，Windows会清理保留的存储，为其他进程（例如更新Windows）释放空间，以避免由于空间不足而导致的问题。

该功能自1903版开始可用，并且在全新安装后或在新制造的电脑上默认启用。

从2004版开始，Windows 10为部署映像服务和管理（DISM）命令工具发布了新的命令，该命令使你可以确定是否配置了保留存储以及启用或禁用该功能。

```shell
# 状态
DISM.exe /Online /Get-ReservedStorageState
# 关闭
DISM.exe /Online /Set-ReservedStorageState /State:Disabled
# 开启
DISM.exe /Online /Set-ReservedStorageState /State:Enabled
```
