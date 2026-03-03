+++
title = "Splitting Ctrl+Space"
author = ["Victor Ren"]
date = 2026-03-02T00:00:00+08:00
tags = ["Emacs", "keybinding"]
categories = ["Trifle"]
draft = false
+++

Emacs comes with numerous default keybindings, which inevitably conflict with
system-level shortcuts.  For Emacs beginners, resolving these conflicts can be a
significant challenge.

One of the most common conflicts is with the command `set-mark-command`, which
default binding is `C-SPC~(Ctrl+Space)`[^fn:1].  On Windows, `Ctrl+Space` is
often used to toggle between English and Chinese input methods, which can make
`Ctrl+Space` unusable for marking in Emacs.

To resolve this issue, you can either use the alternative binding for
`set-mark-command`, `C-@`, or disable the `Ctrl+Space` functionality in
Windows.  Additionally, since the spacebar can
be operated easily with either thumb, there is a "third option": you can
configure the `Ctrl+Space` so that one side `Ctrl+Space` is reserved for
Windows, while the other side `Ctrl+Space` is available for Emacs or other
applications.  This is the solution I adopted because it allows me to
maintain my habit of using both thumbs for operation.

In the latest version of Windows 11, you can achieve this as follows:

1.  Go to `Settings > Time & Language > Language & region > Options > Microsoft
       Pinyin > Keys`, and uncheck the `Ctrl + Space` box while leaving `Shift`
    checked.
    ![](/ox-hugo/disable_Ctrl+Space.png)

2.  Open PowerToys (install it if you don't already have it). In the `Keyboard
       Manager` section, add a key remapping: map `Ctrl(Left) + Space` to `Shift` or
    `Win(Left) + Space`.
    ![](/ox-hugo/remap_shortcuts.png)

For older versions of Windows, where there's a UI bug preventing such
changes&nbsp;[^fn:2], you can solve this by manually editing the registry:

1.  Navigate to `HKEY_CURRENT_USER\Control Panel\Input Method\Hot
       Keys\00000010`. Modify the value of `Key Modifiers` to `02 80 00 00`, or `02
       40 00 00`&nbsp;[^fn:3].
2.  If you want to apply this change for all new users, make the same
    modification under `HKEY_USERS\.DEFAULT\Control Panel\Input Method\Hot
       Keys\00000010`.

After making these changes, restart your computer to take effect.

It seems that only `Ctrl+Space` can be “split,” as other key combinations are
naturally easier to reach with one hand. For example, `Ctrl+a` inherently implies
`Right Ctrl+a`.

[^fn:1]: Emacs uses unique convention for keybinngs, where `C-SPC` is shorthand for
    `Ctrl+Space`.  This article adopts the common convention for now.
[^fn:2]: <https://superuser.com/questions/327479/ctrl-space-always-toggles-chinese-ime-windows-7>
[^fn:3]: 02 indicates Ctrl, 80 indicates the left side, and 40 indicates the right side.
    <https://learn.microsoft.com/en-us/windows/win32/tsf/tf-mod--constants>
