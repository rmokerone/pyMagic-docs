.. _quickref:

pyMagic 快速参考
===============================

.. image:: http://micropython.org/resources/pybv10-pinout.jpg
    :alt: PYBv1.0 pinout
    :width: 700px

常用控制
---------------------

See :mod:`pyb`. ::

    import pyb

    pyb.delay(50) # 等待50ms
    pyb.millis() # 从bootup到当前的ms数
    pyb.repl_uart(pyb.UART(1, 9600)) # duplicate REPL on UART(1)
    pyb.wfi() # 暂停CPU,等待中断
    pyb.freq() # 获取CPU 和 bus 频率
    pyb.freq(60000000) # 设置 CPU 频率到 60MHz
    pyb.stop() # 停止 CPU, 等待外部中断

LEDs
----

See :ref:`pyb.LED <pyb.LED>`. ::

    from pyb import LED

    led = LED(1) # 红色led
    led.toggle()
    led.on()
    led.off()

Pins and GPIO
-------------

See :ref:`pyb.Pin <pyb.Pin>`. ::

    from pyb import Pin

    p_out = Pin('X1', Pin.OUT_PP)
    p_out.high()
    p_out.low()

    p_in = Pin('X2', Pin.IN, Pin.PULL_UP)
    p_in.value() # 获得引脚电平值, 0 or 1

舵机控制(Servo control)
-----------------------

See :ref:`pyb.Servo <pyb.Servo>`. ::

    from pyb import Servo

    s1 = Servo(1) # 舵机在位置 1 (X1, VIN, GND)
    s1.angle(45) # 移动到 45 度
    s1.angle(-60, 1500) # 在1500ms内移动到 -60 度
    s1.speed(50) # 连续旋转舵机

外部中断(External interrupts)
-----------------------------

See :ref:`pyb.ExtInt <pyb.ExtInt>`. ::

    from pyb import Pin, ExtInt

    callback = lambda e: print("intr")
    ext = ExtInt(Pin('Y1'), ExtInt.IRQ_RISING, Pin.PULL_NONE, callback)

定时器(Timers)
--------------

See :ref:`pyb.Timer <pyb.Timer>`. ::

    from pyb import Timer

    tim = Timer(1, freq=1000)
    tim.counter() # 获得计数值
    tim.freq(0.5) # 0.5 Hz
    tim.callback(lambda t: pyb.LED(1).toggle())

PWM (脉冲宽度调制)
------------------------------------------

See :ref:`pyb.Pin <pyb.Pin>` and :ref:`pyb.Timer <pyb.Timer>`. ::

    from pyb import Pin, Timer

    p = Pin('X1') # X1引脚具有 TIM2, CH1功能
    tim = Timer(2, freq=1000)
    ch = tim.channel(1, Timer.PWM, pin=p)
    ch.pulse_width_percent(50)

ADC (模拟转数字)
---------------------------------------------

See :ref:`pyb.Pin <pyb.Pin>` and :ref:`pyb.ADC <pyb.ADC>`. ::

    from pyb import Pin, ADC

    adc = ADC(Pin('X19'))
    adc.read() # 读数, 0-4095

DAC (数字转模拟)
---------------------------------------------

See :ref:`pyb.Pin <pyb.Pin>` and :ref:`pyb.DAC <pyb.DAC>`. ::

    from pyb import Pin, DAC

    dac = DAC(Pin('X5'))
    dac.write(120) # 输出0 到 255

UART (串行总线)
--------------------------

See :ref:`pyb.UART <pyb.UART>`. ::

    from pyb import UART

    uart = UART(1, 9600)
    uart.write('hello')
    uart.read(5) # 最多读取5个字节

SPI总线 (SPI bus)
-----------------

See :ref:`pyb.SPI <pyb.SPI>`. ::

    from pyb import SPI

    spi = SPI(1, SPI.MASTER, baudrate=200000, polarity=1, phase=0)
    spi.send('hello')
    spi.recv(5) # 在该总线上接收5个字节
    spi.send_recv('hello') # 发送并接收5个字节

I2C总线 (I2C bus)
-----------------

See :ref:`pyb.I2C <pyb.I2C>`. ::

    from pyb import I2C

    i2c = I2C(1, I2C.MASTER, baudrate=100000)
    i2c.scan() # 返回从机地址列表
    i2c.send('hello', 0x42) # 发送5个字节到地址为0x42的从机
    i2c.recv(5, 0x42) # 从从机接收5个字节
    i2c.mem_read(2, 0x42, 0x10) # 从地址为0x42的从机 去读内存地址0x10的两个字节
    i2c.mem_write('xy', 0x42, 0x10) # 写两个字节到地址为0x42的从机的0x10内存位置
