# No.106 Windows：将Windows Terminal添加到鼠标右键

## 导读

> 非常实用的小技巧。

1. 下载Windows Terminal图标，右键另存为。

    ![WindowsTerminal图标](../../pics/No.106%20Windows：Terminal.ico "Windows Terminal图标")

2. 保存至'C:\Users\Administrator\AppData\Local\Terminal\Terminal.ico'。

3. win+R输入'regedit'，打开注册表。

4. 找到'HKEY_CLASSES_ROOT\Directory\Background\shell'目录，在此目录下操作。

5. 新建项'wt'，双击右边'默认'，更改右键文字显示内容'Windows Terminal Here'。

6. 新建字符串值'Icon'，更改程序图标路径'C:\Users\Administrator\AppData\Local\Terminal\Terminal.ico'，用于添加程序图标。

7. 在'wt'项中新建子项'command'，双击右边'默认'，更改程序路径'C:\Users\Administrator\AppData\Local\Microsoft\WindowsApps\wt.exe'。
