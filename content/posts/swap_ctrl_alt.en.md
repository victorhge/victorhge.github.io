+++
title = "Swapping Ctrl and Alt Keys"
date = 2026-02-25T20:59:00+08:00
tags = ["Emacs"]
categories = ["Tips"]
draft = false
creator = "Emacs 30.2 (Org mode 9.7.39 + ox-hugo)"
+++

This is an essential configuration for all the computers I use, all because of
Emacs.

Emacs was originally developed as a set of macros for the TECO editor ([Editing
MACroS](https://blog.djmnet.org/2008/08/05/origin-of-emacs/)). At that time, the keyboard layout used by the developers placed the
Ctrl key next to the spacebar[^fn:1], making it convenient for operation with
the thumbs. For some reason, standard keyboards that became widespread with
personal computers moved the Ctrl key to the outermost position, which forces
usage with the pinky finger. This design easily fatigues the pinky.

Therefore, I choose to swap the Ctrl and Alt keys, returning the Ctrl key
to a thumb-operable position, alleviating pinky strain and significantly
improving efficiency.

Here’s how I do it:

-   Windows: Using [SharpKeys](https://github.com/randyrants/sharpkeys) for key remapping.
-   Linux (GNOME desktop): GNOME Tweak Tool makes this easy to configure[^fn:2].

This is a "once-you-use-it-you-can't-go-back" configuration.  It also has a
'hidden feature' - driving others crazy when they try to use my keyboard.

[^fn:1]: Knight Keyboard <http://xahlee.info/kbd/knight_keyboard.html>
[^fn:2]: GNOME Treak Tool <https://askubuntu.com/questions/885045/how-to-swap-ctrl-and-alt-keys-in-ubuntu-16-04/885047>
