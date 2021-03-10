# No.118 爬虫：Frida基础操作

## 导读

> Frida相关的基础操作，包含adb、frida、objection的基本命令。

- ADB常用命令

```shell
# 查看当前连接设备
adb devices

# 多个设备时指定设备
adb -s 设备号 其他指令
# adb -s 127.0.0.1:21503 shell
# adb -s FA6AE0309067 shell
# 127.0.0.1:5555 蓝叠
# 127.0.0.1:7555 MUMU模拟器
# 127.0.0.1:62001 夜游神模拟器
# 127.0.0.1:21503 逍遥模拟器

# 查看Android处理器架构
adb shell getprop ro.product.cpu.abi

# 安装APP
adb install xxx.apk

# 安装APP,已经存在,覆盖安装
adb install -r xxx.apk

# 卸载APP
adb uninstall app包名

# 卸载APP,保留数据
adb uninstall -k app包名

# 往手机传递文件
adb push 文件名 手机路径

# 从手机端获取文件
adb pull 手机路径/文件名

# 移动文件
mv /sdcard/Download/frida-server-12.8.0 /data/local/tmp/

# 修改文件权限
chmod 777 /data/local/tmp/frida-server-12.8.0

# 查看日志
adb logcat

# 清日志
adb logcat -c

# 手机端安装的所有app包名
adb shell pm list packages

# 查看当前包名和主Activity
adb shell dumpsys window | findstr mCurrentFocus

# 启动APP
adb shell am start 包名/主Activity
# adb shell am start com.autonavi.minimap/com.autonavi.map.activity.NewMapActivity

# 关闭App
adb shell am force-stop 包名
# adb shell am force-stop com.autonavi.minimap

# 屏幕截图
adb shell screencap 手机路径/xxx.png

# 录制视频
adb shell screenrecord 手机路径/xxx.mp4

```

- Frida

```shell
# 启动frida-server(模拟器)
./data/local/tmp/frida-server-12.8.0-android-x86

# 启动frida-server(Pixel真机)
./data/local/tmp/frida-server-12.8.0-android-arm64

# 转发端口
adb forward tcp:27042 tcp:27042
adb forward tcp:27043 tcp:27043

# 列举出来所有连接到电脑上的设备
frida-ls-devices

# 连接到指定设备
frida-ps -D tcp

# 列举出来设备上的所有进程
frida-ps -U

# 列举出来设备上的所有应用程序
frida-ps -Ua

# 列举出来设备上的所有已安装应用程序和对应的名字
frida-ps -Uai

# 跟踪某个函数
frida-trace -U -f Name -i "函数名"
# frida-trace -U -f com.autonavi.minimap -i "getRequestParams"

# 跟踪某个方法
frida-trace -U -f Name -m "方法名"
# frida-trace -U -f com.autonavi.minimap -m "MapLoader"

```

- objection常用命令

```shell
# 将objection注入应用
objection -g com.autonavi.minimap explore

# 查看内存中加载的库
memory list modules

# 查看库的导出函数
memory list exports libssl.so

# 将库的导出函数保存到json文件中
memory list exports libart.so --json libart.json

# 内存堆搜索类的实例
android heap search instances com.autonavi.jni.ae.gmap.maploader.MapLoader

# 调用实例的方法
android heap execute 0x2526 getRequestParams

# 启动activity
android intent launch_activity com.autonavi.map.activity.NewMapActivity

# 查看当前可用的activity
android hooking list activities

# 列出内存中所有的类
android hooking list classes

# 内存中搜索所有的类
android hooking search classes MapLoader

# 内存中搜索所有的方法
android hooking search methods getRequestParams

# 列出类的所有方法
android hooking list class_methods com.autonavi.jni.ae.gmap.maploader.MapLoader

# 直接生成hook代码
android hooking generate simple com.autonavi.jni.ae.gmap.maploader.MapLoader

# hook类的所有方法
android hooking watch class com.autonavi.jni.ae.gmap.maploader.MapLoader

# objection当前的Hook数
jobs list

# 查看方法的参数、返回值和调用栈
android hooking watch class_method com.autonavi.jni.ae.gmap.maploader.MapLoader.getRequestParams --dump-args --dump-return --dump-backtrace

# hook类的所有重载
android hooking watch class_method com.autonavi.jni.ae.gmap.maploader.MapLoader.$init --dump-args

```
