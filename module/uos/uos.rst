uos module
===================================

The ``uos`` module contains file system operations.

We do not recommend  operate ``spiflash`` directly. We ported the ``spiffs`` file system to MaixPy and we can manage the files on the development board via spiffs.

Format spiffs.

.. code-block:: code

                uos.formatfs()

Create a file and write the data.

The first parameter file name, the second parameter is the file offset(Note that the offset is the file offset , not the offset in the flash). The third parameter is whencd, and the fourth parameter is the write data buffer which type is bytearray.

.. code-block:: bash

                buf=bytearray(320)
                ...  #buf operation
                uos.write("test",0,0,buf)

List the files, the former lists only all the file names, and the latter lists the file names together with the size which unit is bytes.

.. code-block:: bash

                uos.ls()
                uos.listdir()   
				
Read the data.

The first parameter is the file name, the second parameter is file offset, the third parameter is the whence, and the fourth parameter is the read data buffer which type is bytearray.

.. code-block:: bash

                buf=bytearray(320)
                uos.write("/test",0,0,buf)

Remove the file, the parameter is the file name.

.. code-block:: bash

                uos.remove("/test")
				
Rename the file, the first parameter is the old file name, and the second parameter is the new file name.

.. code-block:: bash

                uos.rename("/test","new_test")
