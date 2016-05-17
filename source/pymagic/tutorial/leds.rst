点亮LED灯和基本的python语法
=========================================

在pyMagic上能做到的最简单的事情就是点亮板子上面的LED。连接板子，并按照上一节的描述打开中断。
我们在终端解释器上输入如下命令，打开并点亮LED ::

    >>> myled = pyb.LED(1)
    >>> myled.on()
    >>> myled.off()

上面的这些命令打开并关闭了LED。

这些都非常不错，但是我们想自动进行处理。在你最喜欢的文本编辑器中打开 ``main.py`` 。写入或者粘贴如下代码到该文件中去。
如果你是python新手，确保在这个例子中开头缩进正常! ::

    led = pyb.LED(2)
    while True:
        led.toggle()
        pyb.delay(1000)

当你保存，pyMagic上面的红色LED将会亮起大约1秒。使用软复位(CTRL-D)运行这个程序。然后，pyMagic将会重启，你就能够看到绿色的LED在不停的闪烁。你通往
建造邪恶机器人军团的第一步已经成功了! 当你不想让LED闪烁了，你可以通过在终端中执行 CTRL-C 来中断LED闪烁。

这些代码做了什么? 首先我们要了解些术语。Python是一种面向对象的语言。几乎所有的事物在Python都可以用 *class* 来描述，*object* 即为你创建的一个指定类的对象。类具有与其关联的 *methods* 。一个method(也可以称为成员函数)用来与影响和控制object。

代码的第一行创建了一个名称为led的LED对象。在创建这个对象时，必须带有范围为1到4的单个参数，代表pyMagic板子上的4颗LED。 pyb.LED类具有三个重要的成员
函数: on(), off() 和 toggler()。我们使用的另一个函数pyb.delay()表示暂停等待指定的毫秒。首先我们创建了LED对象, 然后创建一个无限循环，在循环中先翻转led，然后等待1s。这样led就会以2s为周期进行闪烁了。

**练习: 尝试改变翻转led的时间间隔，或者点亮不同的LED。**

**练习: 直接链接pyMagic, 创建一个pyb.LED对象，并使用on()方式打开。**

pyMagic Disco
-----------------------

到目前位置你只用的pyMagic上的单颗LED,但是pyMagic上有4颗。让我们为每个LED都创建一个对象，这样我们就能够控制每个LED了。我们通过list comprehension方式创建一个LED对象列表。

    leds = [pyb.LED(i) for i in range(1,5)]

如果你使用1,2,3,4以外的其它数字调用pyb.LED()将导致一个错误。
接下来我们使用一个无限循环，并在循环体里依次翻转各个LED。::

    n = 0
    while True:
      n = (n + 1) % 4
      leds[n].toggle()
      pyb.delay(50)

这里n表示当前是哪个LED，每次循环体执行后,n自动循环到下一个(%为取余符号，让n在0到3中循环)。然后我们获取第n个LED并翻转它。运行之后你应该能够看到
所有的led周期性的亮灭。

你可能发现存在这样一个问题:当你停下脚本，然后再运行, led在前一次运行的位置卡住，这样就会毁掉了我们精心编排的Disco。我们可以使用在程序运行之前将
所有LED关闭或者使用try/finally 方式进行解决。当我们按下CTRL-C, microPython产生了一个VCPInterrupt异常。
异常通常意味着发生了一些错误，你可以使用try命令对异常进行捕获。在这个示例中异常仅仅是用户中断，因此我们并不需要进行错误处理，
只需要告诉MicroPython当我们退出时需要做什么。在finally块中我们进行处理，然后我们使用这些语句来却确保所有的LEDs关闭。完整的代码如下::

    leds = [pyb.LED(i) for i in range(1,5)]
    for l in leds: 
        l.off()

    n = 0
    try:
       while True:
          n = (n + 1) % 4
          leds[n].toggle()
          pyb.delay(50)
    finally:
        for l in leds:
            l.off()

第四颗特殊LED
----------------------

蓝色的LED有些特别。除了on 和 off以外，你也可使用intensity()方式来控制其亮度。使用0到255之间的数字表示亮度如何。
下面的代码展示了呼吸灯的效果::

    led = pyb.LED(4)
    intensity = 0
    while True:
        intensity = (intensity + 1) % 255
        led.intensity(intensity)
        pyb.delay(20)

同样你可以使用intensity()控制其他LEDs,但是对其他LED只有量灭两种效果，其中0是灭，其他非零数为亮。
