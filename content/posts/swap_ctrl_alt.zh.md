+++
title = "交换Ctrl和Alt键"
date = 2026-02-25T20:59:00+08:00
tags = ["Emacs"]
categories = ["Tips"]
draft = false
creator = "Emacs 30.2 (Org mode 9.7.39 + ox-hugo)"
+++

这是我所有所用计算机上的必备配置，全是因为Emacs。

Emacs最初是为TECO编辑器开发的宏程序([Editing MACroS](https://blog.djmnet.org/2008/08/05/origin-of-emacs/))。当时的开发者所用键
盘布局将Ctrl键放在空格键两侧[^fn:1]，便于大拇指操作。不知什么原因，微型机普及
后的标准键盘却将Ctrl键移至最外侧，只能靠小拇指操作。这种设计容易导致小拇指疲
劳。

因此，我选择交换Ctrl和Alt键，让Ctrl键重回大拇指位置，既缓解小拇指负担，又
显著提升效率。

我的具体方法是：

-   Windows：通过[SharpKeys](https://github.com/randyrants/sharpkeys)实现键位重映射。
-   Linux (GNOME桌面)：GNOME Tweak Tool可轻松完成设置[^fn:2]。

这是一个“一旦用上就回不去”的配置。它还有一个“隐藏功能”——让其他尝试操作我键
盘的人抓狂。

脚注：

[^fn:1]: Knight Keyboard <http://xahlee.info/kbd/knight_keyboard.html>
[^fn:2]: GNOME Treak Tool <https://askubuntu.com/questions/885045/how-to-swap-ctrl-and-alt-keys-in-ubuntu-16-04/885047>
