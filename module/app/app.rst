app module
===================================

The ``app`` module includes application software on MaixPy to help users better operate the Sipeed M1.

vi
---

``Vi`` is a terminal editing software that can edit files in the development board through the terminal.

.. code-block:: code

                import app
                app.vi(fileName)

 As shown below,we can write script and execute it directly on the Sipeed M1 development board.

.. code-block:: code
                
                app.vi(“/test.py”)
                ....#edit test.py
                import test
