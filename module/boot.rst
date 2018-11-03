Boot
^^^^^^^^^^^^

.. contents:: Article directory


Boot process
--------

Configu System 
~~~~~~~~~~~~~~~~
During the boot process, Sipeed M1 first initializes the system configuration, such as the system clock, REPL serial port, flash, and spiffs file system.

Import script 
~~~~~~~~~~~~~~~
Execute the image built-in startup script boot.py, which will first import related packages of the platform, such as machine, app, common, etc., and also import MicroPython built-in modules such as gc, os.

Initialize peripherals
~~~~~~~~~~~~~~~~~~~~~~~~~~~
Initialize the peripheral according to the circuit design of the Sipeed M1, such as camera ov2640, lcd display st7789 and RGB lights.

Detect mode
~~~~~~~~~~~~~~~~
Pin 15 is detected. If the pin is high, it will enter test mode. The test mode will initialize the camera ov2640 and display lcd st7789 and let them work. If it is low, skip the test mode.

Boot
~~~~~~~~~~~~
The user-defined initialization script init.py will be detected in the file system. If there is one, the script will be imported. If not, it will enter MaxiPy.

How to enter the terminal
----------------------------
After compiling and burning, we can select the corresponding serial port in the terminal software, configure the serial port baud rate to be 115200, the stop bit to be 1, and the data bit width to be 8. Connect the development board with the terminal software and press the Enter key. If there is a prompt ">>>", congratulations you have entered MaixPy.
