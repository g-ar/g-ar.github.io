.. title: Communicating With Serial Ports Using Emacs
.. slug: communicating-with-serial-ports-using-emacs
.. date: 2017-12-31 22:00:37 UTC+05:30
.. tags: emacs, serial port, linux
.. category: 
.. link: 
.. description: 
.. type: text

Emacs can even be used to communicate with serial ports! Hence, it can replace softwares like minicom, hyperterminal, putty etc. that are used for serial port communication.

Making a connection is simple, 

.. code:: text

    M-x serial-term

enter port name and baud rate, and it's connected!


E.g. In Linux based systems, if we need to connect to a GSM modem, find its port name by listing ``/dev/serial/by-id/``

.. code:: bash

    ls -l /dev/serial/by-id/

which will show the symbolic link to the serial port to be used. Suppose the name is ``/dev/ttyUSB3``

Then, using that name with the default baud rate (9600 bps) connects to the modem. The settings can be changed at runtime without requiring to reconnect to the serial port.

And we may issue the standard AT commands

.. code:: text

    at
    OK
    at+cpin?
    +CPIN: READY

    OK
    at+csq
    +CSQ: 14,99

    OK
    at+cmgf=1
    OK
