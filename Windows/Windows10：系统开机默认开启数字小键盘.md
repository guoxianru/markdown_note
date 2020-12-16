# Windows10：系统开机默认开启数字小键盘

## 导读

> 非常实用的小技巧。

1. win+R输入'regedit'，打开注册表。

2. 按顺序打开 “HKEY_USERS/.DEFAULT/Control Panel/Keyboard”。

3. 双击InitialKeyboardIndicators，进行编辑。

4. 打开编辑窗口后，将原本的数字修改为“2”，然后点击“确定”。

5. 重启电脑试一下效果即可，如果重启后没有自动开启小键盘，请按照以上步骤重新操作一遍，最后把数字修改为“80000002”，然后重启即可。
