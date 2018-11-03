Machine module
===================================

.. contents:: Article directory

The ``machine`` module is designed to allow users to operate peripherals  on the Sipeed M1 development board by using script.The specific functions of the machine module are described below.

Fpioa
-----

``Fpioa`` is mainly used to map the functions of the k210 chip to external pins. The pin function mapping table can be viewed by ``fpioa.h`` in sdk. This module is only responsible for setting the pin function and is generally used before the peripheral is initialized.

Create an fpioa object without passing parameters.

.. code-block:: bash 

                fpioa=machine.fpioa()

Setting pin function.The first parameter is the pin number, and the second parameter is the on-chip peripheral function number. As shown below, the 18th pin is set to GOIOHS0, and 24 corresponds to the GOIOHS0 function.

.. code-block:: bash

                fpioa.set_function(18,24)  



GPIO&&Pin
------------

The ``GPIO`` module is used to get or set the value of the GPIO. Before using GPIO, you need to use fpioa to map its function to the external pin. Unlike GPIOHS, the k210 chip has only 8 GPIOs, so the parameter cannot be greater than 8.

Create gpio object, the first parameter is GPIO number, the second parameter is GPIO mode, including, input(0), pull-up input(1), pull-down input(2) and output(3) , the third parameter is the value of GPIO port, it is valid when the mode is output.

.. code-block:: bash

                gpio=machine.pin(0,0,0)

Get the value of GPIO. When there is no parameter, it directly obtains the value of GPIO. When the parameter is passed, the value of GPIO port is set and  no return.

.. code-block:: bash

                value=gpio.value()  

or

.. code-block:: bash

                gpio.value(1)

There is also a way to set the GPIO value. The parameter is the value of the GPIO port. The function is to set the value of the GPIO port and return the current value of the GPIO port. If you do not pass in a parameter, it will return an error directly.

.. code-block:: bash

                value=gpio.toggle(1)

Timer
-------

``Timer`` is mainly used to create timers and perform corresponding functions.

Create a timer. The following code use the timer0 of channel0 as parameter. For information about the timer, please refer to the data sheet of k210.

.. code-block:: bash

                timer=machine.timer(0,0)

Initialize the timer, the first parameter freq is the number of interrupts per second , the second parameter period is the period of the timer, the third parameter div is the division factor of the timer, and the fourth parameter callback is the interrupt processing function callback of the timer.

It should be noted that when definethe  interrupt handler function ,it needs to pass the timer as a parameter.Otherwise ,it cab not be executed. When freq and period are set at the same time, freq has higher priority. When the div is 0, the default division factor is used, and the timer will automatically start running after using this method.

.. code-block:: bash

                def func(timer):
                        print(test)

                timer.init(10,0,0,func)

Set the interrupt function of the timer.

.. code-block:: bash

                def func1(timer):
                        prrint(test1)

                timer.callback(func1)

Set the timer period. As shown below, set the period of the timer to 10000 counts.

.. code-block:: bash

                timer.period(10000)
                
Set the timer interrupt frequency.As shown below, set the timer interrupt frequency to 50 times per second. Please try not to be too large, and there may be errors.

.. code-block:: bash

                timer.freq(50)

Get the current count value of the timer.

.. code-block:: bash

                timer.value()

Start the timer.

.. code-block:: bash

                timer.start()

Stop the timer.

.. code-block:: bash

                timer.stop()

Restart the timer.

.. code-block:: bash

                timer.restar()

PWM
----

``PWM`` is mainly used for pulse width modulation. It can set the duty cycle width of the pin output. This function requires a timer. Please try not to use the timer channel which is using in this module.

Before creating a pwm object, you need to map the external pin to the pwm output. The following code map the pin 12 to the first output of timer 0. The startup of MaixPy has mapped the pin of the RGB led to the first to third output of timer0 by default. 

.. code-block:: bash

                fpioa=machin.fpioa()
                self.fpioa.set_function(12, 190)

Create a PWM object, the first parameter is the timer , the second parameter is the timer channel, the third parameter is the pwm frequency, the fourth is the pwm duty cycle, and the fifth is the output external pin. .

The following code shows that the PWM uses Timer 0's 0 channel as the output, its frequency is 2000000, the duty cycle is 90%, and the output pin is pin 12。

After creating a pwm object, pwm runs automatically

.. code-block:: bash

                pwm=machine.pwm(0,0,2000000,90,12)

Initialize pwm, the first parameter is the pwm frequency, the second is the pwm duty cycle, and the third is the output external pin.

.. code-block:: bash

                pwm.init(3000000,30,12)

Set the pwm frequency.

.. code-block:: bash

                pwm.freq(4000000)

Set the pwm duty cycle.

.. code-block:: bash

                pwm.duty(80)

Ov2640
------
The ``OV2640`` module is used to drive the OV2640 camera on the Sipeed M1 platform.

Create an ov2640 object, of course, you need to initialize the external pin before creating the object, but at boot time, the pin has been mapped .We can operate the camera directly

.. code-block:: bash

                ov2640=machine.ov2640()

Initialize ov2640. Before initializing, please make sure the camera is installed on the Sipeed M1. If it is not detected that the camera will enter the detection dead loop, the MaxiPy driver will initialize the ov2640 to 320*240 resolution, corresponding to the default lcd resolution size.

.. code-block:: bash

                ov2640.init()


To obtain the camera image, We need to create a buffer . After acquiring the image, you can display it with the lcd.

.. code-block:: bash

                image=bytearray(320*240*2)
                ov2640.get_image(image)

St7789
--------

The ``st7789`` module is used to drive the st7789 display lcd of the Sipeed M1 platform with a resolution of 320*240.

Createa  st7789 object . Similarly ,the pin mapping is already done at boot time.

.. code-block:: bash

                st7789=machine.st7789()

Initialize st7789.

.. code-block:: bash

                st7789.init()

Draw picture with the default resolution  320*240, the parameter is 320*240*2 bytes of image data buffer.

.. code-block:: bash

                st7789.draw_picture_default(buf)

It can be used with ov2640 for image display.

.. code-block:: bash 

                image=bytearray(320*240*2)
                while(1):
                        ov2640.get_image(image)
                        lcd.draw_picture_default(image)
                        
Use st7789 to draw picture, the first parameter is the x coordinate , the second parameter is the y coordinate , the third parameter is the width of the image, the fourth parameter is the height of the image, the fifth The parameter is the image data buffer.


.. code-block:: bash

                st7789.draw_picture(0,0,320,240,buf)

Use st7789 to draw a string. The first parameter is the x coordinate , the second parameter is the y coordinate , and the third parameter is the string.

.. code-block:: bash

                st7789.draw_string(0,0,"hello world")

Ws2812
------

``Ws2812`` is a low power RGB led with integrated current control chip

Create ws2812 object

.. code-block:: bash

                ws2812=machine.ws2812()

Initialize ws2812.

Ws2812 needs to use GPIOHS for data communication, so before using ws2812, we need to map GPIOHS to the pin, as shown below, map pin 20 to GPIOHS20.

The first parameter is the GPIOHS number , and the second parameter is the external pin .

.. code-block:: bash

                fpioa=machine.fpioa()
                fpioa.set_function(20,44)
                ws2812.init(20,44)

Lights up a single light.

The parameters are R, G, and B components, and each component has a maximum value of 255.

.. code-block:: bash
        
                ws2812.set_RGB(255,255,255)

Lights up multiple lights.

Similarly to set_RGB，the last parameter is the number of lights .

.. code-block:: bash

                ws2812.set_RGB_num(255,255,255,4)


Zmodem
------

``Zmodem`` is a tool for file transfer between PC and development board.  You can use the rz function to get the files on the PC. The prerequisite is that the terminal software supports the zmodem protocol. It is recommended to use xshell or SRC.

Get the PC file by using the following command.

.. code-block:: bash

                machine.zmodem.rz()

Spiflsah
--------

``Spiflsah`` is used to directly operate the nor flash of the development board, such as read, write, and erase.

Create a spiflash object.

.. code-block:: bash

                spiflash=machine.spiflash()     

Initialize the flash.

.. code-block:: bash

                spiflash.init()

Read flash, the first parameter flash read address, the second parameter is the data storage buffer.

As shown below, first create a buffer to store the read data, and then use the read method to store the read data in the buf.

.. code-block:: bash

                buf=bytearray(320)
                spiflash.read(0x100000,buf)

Write flash, the first parameter flash write address, the second parameter is write data buffer.

As shown below, first create a buffer to store the write data, and then use the write method to write the data in the buf to the flash.

.. code-block:: bash

                buf=bytearray(320)
                spiflash.write(0x100000,buf)

Erase flash, the parameter is the erase address, and each operattion erase 4kb data.

.. code-block:: bash

                spiflash.erase(0x100000)

