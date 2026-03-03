+++
title = "劈开 Ctrl+Space"
author = ["Victor Ren"]
date = 2026-03-02T00:00:00+08:00
tags = ["Emacs", "keybinding"]
categories = ["Trifle"]
draft = false
+++

Emacs 拥有大量默认快捷键绑定，不可避免地会与系统快捷键发生冲突，对于 Emacs 新手
来说，如何解决这些冲突是一项重大挑战。

其中最常见的一个是命令 `set-mark-command` ，默认键绑定是
`C-SPC(Ctrl+Space)`&nbsp;[^fn:1]。在 Windows 系统中， `Ctrl+Space` 默认用于切换中英文
输入法，这个冲突会导致在 Windows 下的 Emacs 无法使用 `Ctrl+Space` 进行标记设置。

要解决这个问题，可以选择使用 `set-mark-command` 的备用快捷键 `C-@` ，或者禁用
Windows 的 `Ctrl+Space` 功能。此外，由于空格键左右手操作同样方便，还有“第三选
择: 把 `Ctrl+Space` 一分为二，一侧的 `Ctrl+Space` 留给 Windows，另一侧释放出来供
Emacs 或其他应用程序使用。这正是我采用的方案，因为这样我就可以保留用两个大拇指操
作的习惯。

在最新的 Windows 11 下可以这么做：

1.  找到 `Settings > Time & Language > Language & region > Options > Microsoft
       Pinyin > Keys` , 取消勾选 `Ctrl + Space` ，保留勾选 `Shift` 。
    ![](/ox-hugo/disable_Ctrl+Space.png)

2.  打开 PowerToys (没有的话先安装）， 在 Keyboard Manager 功能中，添加一个快
    捷键重映射：将 `Ctrl(Left) + Space` 映射为 `Shift` 或者 `Win(Left) + Space` 。
    ![](/ox-hugo/remap_shortcuts.png)

此前的 Windows 版本存在一个 UI 无法更改相关设置的缺陷[^fn:2]，可以通过修改注册表解决：

1.  找到 `HKEY_CURRENT_USER\Control Panel\Input Method\Hot Keys\00000010` 条目，
    将其中的 `Key Modifiers` 修改为 `02 80 00 00` , 或者 `02 40 00 00`&nbsp;[^fn:3]。
2.  如果希望将更改应用于所有新用户，还需要在 `HKEY_USERS\.DEFAULT\Control Panel\Input
       Method\Hot Keys\00000010` 中进行同样的修改。

修改完成后，重启电脑即可生效。

似乎只有 `Ctrl+Space` 才能“劈开”， 其他的键天然的只对一只手来说顺手，比如
`Ctrl+a` 就已经隐含着 `Right Ctrl+a` 。


## 脚注 {#脚注}

[^fn:1]: Emacs 有独特的键位的缩写习惯，用 `C-SPC` 代表 `Ctrl+Space` 。本文暂且保留常用习惯。
[^fn:2]: <https://superuser.com/questions/327479/ctrl-space-always-toggles-chinese-ime-windows-7>
[^fn:3]: 02 指Ctrl, 80 指左侧，40指右侧
    <https://learn.microsoft.com/en-us/windows/win32/tsf/tf-mod--constants>
