---
title: Xrandr — Linux螢幕方向設定教學
date: 2020-09-13T17:01:22+08:00
# tags: Linux
---

不知道大家有沒有過打開ubuntu之後，發現整個螢幕的顯示方向從原本的水平變成垂直，或者是方向整個不對的經驗。像是下面這樣…

![fdfd](https://i.imgur.com/aKI7tGO.png)
<!-- more -->
在查過一些網路上的資料之後，這個原因或許是因為筆電裡面內建的陀螺儀在搞鬼。

如果要關掉ubuntu中內建的自動旋轉螢幕方向，可以點右上角的選單(本圖的桌面環境為gnome，其他的像KDE、Unity可能要再另外找找看)，紅色圈起來的那個按鈕就是控制ubuntu自動旋轉螢幕方向的元兇了!


但上面的方法只能預防這類的情況發生，如果要「治療」這個狀況，貌似只能透過terminal指令來更改螢幕方向。所以今天要介紹的是linux內建的指令xrandr。

-------------------------------------------

XRandR - (X Rotate and Reflect Extension)，顧名思義就是用來控制旋轉以及大小的。

要把螢幕方向給他好好地轉回來，就直接打開terminal(快捷鍵:Alt+T)，輸入下面的指令。

```
$ xrandr -o [the orientation螢幕方向]# 恢復正常
$ xrandr -o normal# 往左轉180度
$ xrandr -o left# 往右轉180度
$ xrandr -o right# 直接翻轉180度
$ xrandr -o inverted
```

查了一下arch linux官方有關於xrandr的文件之後，有一些更詳細的用法。

```
# 直接顯示目前電腦有接上的輸出裝置的資訊
$ xrandr# 調整輸出裝置的設定
$ xrandr --output [輸出裝置，ex:HDMI-1] --mode [解析度，ex:1920x1080]
--rate [更新頻率，ex:60]
```

其他還想知道更深入的用法可以去看看arch linux官方提供的文件。

https://wiki.archlinux.org/index.php/Xrandr

還有這篇x.org的manual，內容比上面arch linux提供的還要詳盡。

https://www.x.org/releases/X11R7.5/doc/man/man1/xrandr.1.html