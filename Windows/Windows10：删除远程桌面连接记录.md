# Windows10：删除远程桌面连接记录

## 导读

> 非常实用的小技巧。

1. win+R输入'regedit'，打开注册表。

2. 按顺序打开 “HKEY_CURRENT_USER-/Software/Microsoft/Terminal Server Client/Default”。

3. 在Default的右侧可以看到远程连接记录的IP地址。

4. 鼠标右键想要删除的远程连接记录，点击删除，会弹出框出来，点击确定就删除了。
