.. _general
pyMagic常用信息
=====================================

按键位置
---------
pyMagic上面有三个按键,
左上角的为usr按键,进入DFU模式时用到.
右上角的为reset按键,进行复位时用到.
右下角的为user按键,用来编程和进入不同的启动模式.

本地文件系统和SD卡
----------------------------

pyMagic中具有一个非常小的文件系统(驱动),称为 ``/flash``,
这个文件系统是存储在微控制器的flash内存中. 如果micro SD卡插入了卡槽, 
就会生成一个名为``/sd``的文件系统. 

当pyMagic启动时，需要选择从哪个文件系统启动. 如果没有SD卡，默认使用内部
文件系统 ``/flash`` 作为启动文件系统,　如果有，则使用SD卡 ``/sd``.

启动文件系统用于2件事情: 是被查找的 ``boot.py`` 和 ``main.py`` 所在的文件系统,
是你通过USB线连接到电脑上能看到的文件系统.

该文件系统能够在你的电脑上以移动硬盘的形式出现. 你可以保存文件到这个硬盘,
并可以编辑 ``boot.py`` 和 ``main.py``.

*记得在你复位你的pyMagic前一定要弹出该USB设备(在Linux上, 卸载)*

启动模式(Boot modes)
--------------------

如果你正常上电启动, 或者按下复位按键(reset button), pyMagic将进入
标准模式(standard mode): ``boot.py`` 文件将首先执行, 接着将配置USB设置,
然后再运行 ``main.py``.

你可以通过在pyMagic上电过程中按住用户按键(user switch)来重写启动顺序.
按住用户按键(user switch)然后按下复位, 当你持续按着用户按键(user switch)
不放开, LED灯会交替闪烁。当LEDs闪烁到你想要进入的模式，放开用户按键(user switch),
然后选定模式的LEDs将快速闪烁，接着pyMagic将启动.

具有的模式:

1. 仅绿色LED, *标准启动*: 运行 ``boot.py`` 然后再运行 ``main.py``.
2. 仅橙色LED, *安全启动*: 启动过程不运行任何脚本文件
3. 绿色LED和橙色LED一起亮, *文件系统重置*: 复位文件系统到出厂设置,
   然后以安全模式进行启动.

如果你的文件系统出现问题，以mode 3的方式启动进行修复.
如果在你电脑上进行文件系统恢复失败，你可以尝试插到USB充电器或者用其它
没有数据链接的USB供电方式重复上述操作进行尝试.

错误: LEDs闪烁
---------------------

目前你可能见到的错误有两种:

1. 如果红色和绿色LED交替闪烁, 表示一个python脚本出现问题
   (eg, ``main.py``), 使用REPL进行调试.
2. 如果4个LEDs循环h缓慢亮灭, 表示出现了硬件错误，并且这是无法恢复的,
   这时你需要pyMagic的客服联系，尝试寄回修复.

