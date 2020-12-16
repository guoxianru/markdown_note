# 将Sublime添加到鼠标右键

## 导读

> 非常实用的小技巧。

1. win+R输入'regedit'，打开注册表。

2. 找到'HKEY_CLASSES_ROOT/*/shell'目录，在此目录下操作。

3. 新建项'sublime'，双击右边'默认'，更改右键文字显示内容。

4. 新建字符串值'Icon'，更改程序图标路径'C:\ProgramData\Sublime Text\sublime.exe'，用于添加程序图标。

5. 在'sublime'项中新建子项'command'，双击右边'默认'，更改程序路径'C:\ProgramData\Sublime Text\sublime.exe %1'。
