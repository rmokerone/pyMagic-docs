运行第一个程序
=========================

让我们开始在pyMagic上面运行一python程序。毕竟，这是pyMagic的最主要功能。

连接yMagic
-----------------------

将你的pyMagic通过micro USB线连接到PC(Windows, Mac 或 Linux等系统)。板子上面只有
一个Micro USB接口，所以不用担心自己会连接错误。

.. image:: img/pyboard_usb_micro.jpg

当pyMagic连接到你的电脑后，pyMagic就会自动上电并启动进程(boot进程)。绿灯会亮起一秒或
一秒以内，当启动进程完成，绿灯将熄灭。

开启pyMagic USB驱动
-----------------------------

现在你的电脑应该能够识别pyMagic了。下一步怎么做将取决于你的系统:
  - **Windows**: 你的pyMagic将作为一个可移动USB设备出现。Windows将自动弹出一个窗口
    ，或者你可以使用文件管理器访问。

    同时Windos也会将pyMagic识别为一个串口设备,并会自动为这个设备做一些配置。如果确实这样，
    取消这个进程，我们将在下一节说明如何让串口设备工作。

  - **Mac**: 你的pyMagic将做为一个可移动文件出现在桌面上。它的名字可能是"NONAME"。点击它进入
    pyMagic文件夹。

  - **Linux**: 你的pymagic将作为一个可移动媒体出现。在Ubuntu上它将自动挂载，并弹出一个pyMagic
    文件夹的窗口。在其他的Linux发布系统上，pyMagic可能自动挂载，也可能需要你进行手动挂载。
    在terminal上输入 ``lsblk`` 可以查看连接到电脑的设备列表，然后输入 ``mount /dev/sdb1`` (使用你的
    pyMagic名称取代 ``sb1`` )。做这些可能需要root权限:-D。

好的，你现在应该将pyMagic作为USB移动设备连接到了电脑，这时应该能够打开一个窗口(或命令行)看到
pyMagic设备中的文件了。

你现在看到的设备在pyMagic上名为 ``/flash`` ,它应该包括以下四个文件：

* `boot.py <http://micropython.org/resources/fresh-pyboard/boot.py>`_ -- pyMagic启动时执行的python文件。它用来设置pyMagic的各种配置参数。

* `main.py <http://micropython.org/resources/fresh-pyboard/main.py>`_ -- 包含你python程序的主要文件。它将在 ``boot.py`` 执行后执行。

* `README.txt <http://micropython.org/resources/fresh-pyboard/README.txt>`_ -- 包含一些刚学习pyMagic能用到的基本信息。

* `pybcdc.inf <http://micropython.org/resources/fresh-pyboard/pybcdc.inf>`_ -- 将pyMagic作为串口设备所需要的windows驱动。下一节会仔细介绍。

编辑 ``main.py``
-------------------

现在， 在文本编辑器中打开 ``main.py`` 文件开始写我们的python程序。
在Windows中你可以用记事本或者其他的编辑器，在Mac或Linux上，请自行选择。
将这个文件打开你可以看到如下一行代码 ::

    # main.py -- put your code here!

该行以一个 # 字符开头，表示这样被 *注释* 了。因此该行将不起任何实际意义， 因此
可以在注释符后面添加些程序的注释。

在 ``main.py`` 中添加如下两行代码, 使其看起来和下面一样::

    # main.py -- put your code here!
    import pyb
    pyb.LED(4).on()

我们写的第一行代码的意思，表示我们想使用 ``pyb`` 模块(module)。
这个模块包含了所有控制pyMagic的功能函数和类。

我们写的第二行是打开蓝色的灯: 它先从 ``pyb`` 模块中获取到 ``LED`` 类， 创建
一个序号为4的LED(蓝色的介个)，并将其状态设置为on。 

复位pyMagic
---------------------

为了运行这个小脚本，第一步你应该关闭和保存 ``main.py`` 文件，
然后弹出(或卸载)这个pyMagic USB设备。这些操作和你操作正常的USB移动硬盘是一样的。

当设备被安全弹出/卸载后，就到了最fun的部分:
按住pyMagic的RST按键去复位并运行你的程序。RST按键在micro USB口的右边。

当你按下RST按键后， 绿色LED将会快速闪烁，然后蓝色的LED将会量起并保持状态。

恭喜! 你已经完成了你的第一个pyMagic程序的编写和运行。
