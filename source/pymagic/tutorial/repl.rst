获得pyMagic的REPL终端
=================================

REPL 表示读取解释打印循环(就是一个即时解释器)，正如其名称所示，你可以通过
pyMagic的终端访问pyMagic。使用即时解释终端是到目前位置，最简易测试代码和运行
命令的方式。也就是说，除了在 ``main.py`` 中你可以写代码也可以在REPL中写。

为了使用REPL，你必须将pyMagic作为USB串口设备连接到电脑。
如何操作取决于你的电脑系统.

Windows
-------

为了使用USB串口设备你需要安装pyMagic的驱动。驱动在pyMagic的USB移动设备中，
名称为 ``pybcdc.inf`` 。

为了安装这个设备，你应该打开你系统上的硬件设备管理器，并在列表中找到pyboard。
(它应该带着一个黄色的警示符号，因为它现在还没有正确安装),右键点击pyboard设备，
选择属性，然后安装驱动。你应该选择手动安装，(不要使用Windows系统的自动安装), 
浏览到pyMagic的USB设备，然后选择。就能够正常安装了。安装完成后，返回到硬件设备
管理器，看下COM口是多少(例如COM4)。
更全面的操作可以阅读查看`Guide for pyboard on Windows (PDF) <http://micropython.org/resources/Micro-Python-Windows-setup.pdf>`_.
如果在安装驱动上存在问题请参照这份指南。

你现在应该运行一个终端设备。如果你已经安装HyperTerminal，你可以继续使用，或者你可以从
这里免费下载安装PuTTY：
`putty.exe <http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html>`_.
使用串口你必须提供上一步中找到的端口号(COM*)。对于PuTTY， 点击窗口左侧的 ``Session``,
然后点击 "Serial" 选择按钮，然后在 "Serial Line"框中输入你的COM端口号(例如COM4)。
最后点击"Open"按钮。

Mac OS X
--------

打开终端并运行::

    screen /dev/tty.usbmodem*
    
当你结束并想退出时，输入 CTRL-A CTRL-\\。

ps:tty.usbmodem*中*号的内容具体为多少可以使用下面的方式( ``邵解放。（2898629332)提供``) 查看:
 1. 打开终端进入/dev/目录::
     cd /dev/
 2. 将当前全部设备名保存到 ~/before.txt文件::
     ls > ~/before.txt
 3. 插入pyMagic, 将全部设备名保存到 ~/after.txt文件::
     ls > ~/after.txt
 4. 比较插入前和插入后的全部设备名::
     diff ~/before.txt ~/after.txt
 5. 找到tty开头的设备::
     例如: tty.usbmodem1422
 6. 执行命令连接到设备
     sudo screen /dev/tty.usbmodem1422

Linux
-----

打开终端并运行::

    screen /dev/ttyACM0
    
你可以尝试使用 ``picocom`` 或 ``minicom`` 代替screen. 你可以使用 ``/dev/ttyACM1``
或 ``ttyACM`` 更高的数字。 并且要注意你的访问权限（建议直接使用sudo )。

使用REPL终端
---------------------

现在让我们尝试直接在pyMagic上裕兴microPython代码.

当你的串口程序打开时 (PuTTY, screen, picocom, 等等） 你会看到一个空白的界面上面有一个
跳动的光标。输入Enter你应该能够见到一个microPython的提示符，例如 ``>>>`` 。 使用下面的
必要测试来确定串口是否正常工作::

    >>> print("hello pyboard!")
    hello pyboard!

如上， ``>>>`` 字符串不需要你进行输入。它们在那里只是告诉你应该其后进行输入。在最后，
一旦你输入了 ``print("hello pyboard!")`` 并按下了回车，你screen上的输出结果应该像
上面一样。

如果你了解一些python的语句你可以在这里尝试些基本的命令。

如果其没有正常工作，你可以进行前面提到硬件复位或软件复位;

继续并尝试输入一些其他的命令。例如::

    >>> pyb.LED(1).on()
    >>> pyb.LED(2).on()
    >>> 1 + 2
    3
    >>> 1 / 2
    0.5
    >>> 20 * 'py'
    'pypypypypypypypypypypypypypypypypypypypy'

复位pyMagic
-------------------

如果发生了一些错误，你可以通过两种方式对pyMagic进行复位。第一种方式是软复位:在终端上按下 CTRL-D。
你能够看到一些如下的提示 ::

    >>> 
    PYB: sync filesystems
    PYB: soft reboot
    Micro Python v1.0 on 2014-05-03; PYBv1.0 with STM32F405RG
    Type "help()" for more information.
    >>>

如果这个没有正常工作，你也可以使用通过按下RST按键进行硬件复位。但是这样无论你使用什么软件，
你的终端将会断开。

如果你准备进行硬件复位，强烈建议关闭你的中断程序，弹出/卸载你的pyMagic设备。

