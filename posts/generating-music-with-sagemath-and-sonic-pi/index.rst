.. title: Generating Music With SageMath And Sonic Pi
.. slug: generating-music-with-sagemath-and-sonic-pi
.. date: 2017-10-20 15:23:44 UTC+05:30
.. tags: python, sage, OSC, Sonic Pi
.. category: 
.. link: 
.. description: 
.. type: text

In this post, we will see how to generate music with Python/SageMath and Sonic Pi. 
Sonic Pi itself is quite nice to make music, but python is an attractive option due to large number of math libraries written for it.
So, here are the steps:

1 Install PyOSC
---------------

Open Sage subshell

.. code:: bash
    :number-lines: 1

    sage -sh

Install PyOSC library

.. code:: python
    :number-lines: 1

    pip install PyOSC

2 Run Sonic Pi and sync with an OSC URL
---------------------------------------

.. code:: ruby
    :number-lines: 1

    use_synth :piano

    live_loop :foo do
      use_real_time
      a, b = sync "/osc/trigger/play"
      play_chord [a, b]
    end

3 Pass messages to the OSC URL and play notes
---------------------------------------------

.. code:: python
    :number-lines: 1

    import OSC
    from time import sleep
    from math import pi, cos, sin

    addr = ('localhost', 4559)
    c = OSC.OSCClient()
    c.connect(addr)

    def send_message(url, *args):
        msg = OSC.OSCMessage()
        msg.setAddress(url)
        for l in args:
            msg.append(l)
        c.send(msg)

    for x in range(1,10):
        for y in range(1,20):
            send_message("/trigger/play", float(x*10), float(y*5)) # float's not required in python, but throws error in Sage without it
            sleep(0.1)
