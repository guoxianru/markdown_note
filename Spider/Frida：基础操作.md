# Frida：基础操作

## 导读

> Frida常用命令。

- ADB常用命令

```shell
# 查看Android处理器架构
adb shell getprop ro.product.cpu.abi

# 获取包名和主Activity(手机打开APP状态)
adb shell dumpsys window | findstr mCurrentFocus

# 启动APP(包名/主Activity)
adb shell am start com.autonavi.minimap/com.autonavi.map.activity.NewMapActivity

# 关闭App(包名)
adb shell am force-stop com.autonavi.minimap

# 启动frida-server(模拟器)
./data/local/tmp/frida-server-12.8.0-android-x86

# 启动frida-server(Pixel真机)
./data/local/tmp/frida-server-12.8.0-android-arm64

# 转发端口
adb forward tcp:27042 tcp:27042
adb forward tcp:27043 tcp:27043

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
