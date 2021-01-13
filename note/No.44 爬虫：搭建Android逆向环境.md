# No.44 爬虫：搭建Android逆向环境

## 导读

> Android手机刷机、root、逆向环境搭建。

### 1、基本名词

Bootloader锁、Recovery、Fastboot、高通9008模式、OTA、System分区、A/B分区、全盘加密/加密分区、ADB、Xposed(EdXposed)、Magisk、SafetyNet、Frida。。。

以上知识点请自行了解。

### 2、准备工作

以Google Pixel手机为例，下载按需即可。

1. [下载Google USB驱动程序](https://developer.android.com/studio/run/win-usb)

2. [下载Google Pixel官方镜像文件](https://developers.google.cn/android/images#sailfish)

3. [下载SDK Platform Tools](https://developer.android.com/studio/releases/platform-tools)

4. [下载TWRP](https://twrp.me/google/googlepixel.html)

5. [下载Magisk](https://github.com/topjohnwu/Magisk/releases)

6. [下载EdXposedManager](https://github.com/ElderDrivers/EdXposedManager/releases)

7. [下载Frida](https://github.com/frida/frida/releases)

8. 备份手机资料，退出谷歌账号，关闭指纹识别以及锁屏密码。

### 3、环境搭建

- 手机开启开发者选项，打开USB调试模式。

- 电脑安装Google USB驱动程序。

- 电脑将adb添加到环境变量中。

- 解BL锁

```shell
# 重启进入bootloader
adb reboot bootloader

# 查看连接情况
fastboot devices

# 解锁手机
fastboot oem unlock

# 检查是否已经解锁
fastboot oem device-info
# Device unlocked: true 则表示已经解锁。

# 重启手机
fastboot reboot

```

- 刷机

解压镜像文件，找到flash-all.bat文件并运行。

大概3-5分钟，刷机就成功了，开机跳过所有选项，开启开发者选项，打开USB调试模式继续。

- 更换Recovery

```shell
# 重启进入bootloader
adb reboot bootloader

# 进入临时TWRP
fastboot boot twrp-3.3.0-0-sailfish.img

# 刷入TWRP包
fastboot flash recovery twrp-3.3.0-0-sailfish.img
# 刷完之后不会自动重启，看命令行提示Finished了就行。

# 重启进入Recovery
fastboot reboot recovery
# 重启进入TWRP后，出现安全警告时直接拖动滑块允许修改即可。
```

- 安装Magisk

```shell
# 将下载好的Magisk-v20.4.zip传到手机的sdcard目录中
adb push Magisk-v20.4.zip /sdcard
```

进入TWRP的安装（install）页面，选中刚刚准备好的Magisk的包，刷入它，然后重启。

重启进入系统后，会看到Magisk Manager，用来管理Magisk的，里面可以进行ROOT授权、SafetyNet检测、隐藏Magisk和ROOT、安装/管理Magisk模块之类的操作。

- 安装EdXposed

1. 安装Magisk模块：Riru-Core（作者：Rikka）

2. 安装Magisk模块：EdXposed（作者：SandHook）

- 安装EdXposedManager

将下载好的EdXposedManager-4.5.7-45700.apk安装到手机。

- 安装Frida

```shell
# 推送文件
adb push frida-server-12.8.0-android-arm64 /data/local/tmp

# 修改权限
chmod 777 /data/local/tmp/frida-server-12.8.0-android-arm64

# 开启服务
./data/local/tmp/frida-server-12.8.0-android-arm64

# 验证服务
frida-ps -U

```

### 4、其他问题

- WiFi和LTE（4G）除去叉号

```shell
adb shell "settings put global captive_portal_https_url https://captive.v2ex.co/generate_204"
```
