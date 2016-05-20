将pyMagic作为一个USB鼠标使用
=====================================

pyMagic是一个USB设备，因此其也可以配置成一个鼠标，而不是默认配置成一个USB存储设备。

为了使pyMagic作为一个鼠标，首先你需要编辑 ``boot.py`` 文件去改变USB配置。
如果你还没有接触过你的 ``boot.py`` 文件，其内容和下面的类似。::

    # boot.py -- run on boot-up
    # can run arbitrary Python, but best to keep it minimal

    import pyb
    #pyb.main('main.py') # main script to run after this one
    #pyb.usb_mode('CDC+MSC') # act as a serial and a storage device
    #pyb.usb_mode('CDC+HID') # act as a serial device and a mouse

为了开始鼠标模式， 取消文件最后一行代码的注释。如下所示::

    pyb.usb_mode('CDC+HID') # act as a serial device and a mouse

如果你已经修改了你的 ``boot.py`` 文件，则能满足工作的最少量代码如下所示::

    import pyb
    pyb.usb_mode('CDC+HID')

这些代码告诉pyMagic去配置自己当启动时作为CDC (串口) 和 HID (Human Interface device, 
在我们的示例中是鼠标) USB设备。

弹出/卸载 pyMagic设备，并使用RST按键进行复位。
此时，你的电脑将检测到pyMagic并将其视为鼠标!

手动发送鼠标事件
----------------------------

为了能让pyMouse做一些事情，我们需要向PC发送鼠标事件(mouse events)。
首先我们尝试在REPL终端上手动发送鼠标事件。
使用你的串口程序连接pyMagic到电脑，并输入如下命令::

    >>> pyb.hid((0, 10, 0, 0))

你的mouse将向右移动10个像素点! 在上面的命令中，你发送了4个信息: 按键状态， x, y, 和
滚动。数字10 告诉PC，鼠标在x轴方向移动了10个像素点。

让鼠标左右摇摆起来：：

    >>> import math
    >>> def osc(n, d):
    ...   for i in range(n):
    ...     pyb.hid((0, int(20 * math.sin(i / 10)), 0, 0))
    ...     pyb.delay(d)
    ...
    >>> osc(100, 50)

``osc`` 函数的第一个参数是要发送的的鼠标事件(mouse events)数， 第二个参数是鼠标事件间的
时间间隔, 尝试使用不同的数字替代会有什么效果。

**练习: 尝试让鼠标转圆圈.**

使用加速度传感器做个鼠标
-------------------------------------

*注: 目前pyMagic核心板上没有加速度传感器，但taobao上已经推出了加速度传感器模块。*

现在使用加速度传感器让鼠标基于pyMagic的角度来进行移动。下面的代码可以直接在REPL终端上
直接键入，也可以输入到 ``main.py`` 文件来实现。在这里，我们将使用输入到 ``main.py`` 的
方式实现，因为下面我们会学习如果进入安全模式。

当前pyMagic是作为串口USB设备和HID(鼠标)来进行使用。因此我们不能够进入到文件系统去
编辑 ``main.py`` 文件。

同时，你也同能编辑你的 ``boot.py`` 文件以退出HID-mode 并进入normal模式。也就意味着
你看不到USB移动存储设备了。。。

为了避免这个困境，我们需要进入到 *安全模式* 。这在[safe mode tutorial](tut-reset)
进行了说明，但在这里我们再重复一遍相应的指令。

1. 通过USB连接pyMagic,这样pyMagic就能够启动。
2. 按下USR按键。
3. 在按住USR按键的同时，按下并释放RST按键。
4. 此时LED就会进行绿色，橙色，绿色+橙色的循环。
5. 继续按着USR按键直到 *仅有橙色LED* 亮起的时候释放USR按键。
6. 橙色LED将快速闪烁4次，然后变灭。
7. 这样就成功进入到安全模式。

在安全模式， ``boot.py`` 和 ``main.py`` 文件都不会执行。因此pyMagic将以默认配置
启动。 这意味着你现在能通过文件系统访问和边界 ``main.py`` 文件了(保持 ``boot.py`` 
文件不变，因为接下来我们还想将pyMagic做为鼠标进行使用)。

在 ``main.py`` 文件中输入如下代码::

    import pyb

    switch = pyb.Switch()
    accel = pyb.Accel()

    while not switch():
        pyb.hid((0, accel.x(), accel.y(), 0))
        pyb.delay(20)

保存你的文件，弹出/卸载你的pyboard设备，然后使用RST按键对其进行复位。现在pyMagic
能够被电脑识别为HID设备了，此时pyMagic的角度将四处移动鼠标了。做个小游戏，试下
你能不能让鼠标保持静止不懂。

按下USR按键停止鼠标运动。

你能够看到y轴是想法的，这点很容易能够修复:只需要在 ``pyb.hid()`` 那行的表示y坐标的位置
前面加一个符号。

将你的pyMagic恢复成正常模式
--------------------------------

如果你让你的pyMagic就这样保持不变，那下次再插入电脑还是作为鼠标进行使用。如果你想
将pyMagic恢复到正常。你需要进入到安全模式，然后编辑 ``boot.py`` 文件。在 ``boot.py``
文件中，注释掉(在该行的前面添加 # 字符)设置 ``CDC+HID`` 的一行，如下所示::

    #pyb.usb_mode('CDC+HID') # act as a serial device and a mouse

保存你的文件， 弹出/卸载 设备，并重启pyMagic,现在将返回到正常模式。
