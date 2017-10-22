.. title: Generating Music With SageMath And Sonic Pi - Examples - 2
.. slug: generating-music-with-sagemath-and-sonic-pi-examples-2
.. date: 2017-10-22 16:10:26 UTC+05:30
.. tags: python, sage, OSC, Sonic Pi
.. category: 
.. link: 
.. description: 
.. type: text

Blues Loop
----------

An approximate translation of the Blues Loop illustrated at `mathematica.SE <https://mathematica.stackexchange.com/questions/100876/more-elegant-function-construction-for-blues-loop>`_. `(Listen to the Blues Loop <https://soundcloud.com/user-591836524/blues-loop>`_)

Sync the OSC URL in Sonic Pi

.. code:: ruby
    :number-lines: 1

    live_loop :foo do
      use_synth :piano
      use_real_time
      p1, p2, p3, t1, t2 = sync "/osc/trigger/play"
      play :C, pitch: p1, attack: t1, attack_level: 0, sustain: t2-t1, release: t2-t1
      play :C, pitch: p2, attack: t1, attack_level: 0, sustain: t2-t1, release: t2-t1
      play :C, pitch: p3, attack: t1, attack_level: 0, sustain: t2-t1, release: t2-t1
    end

And send the notes from Sage

.. code:: python
    :number-lines: 1

    %python
    import numpy as np
    import OSC
    from time import sleep

    addr = ('localhost', 4559)
    cl = OSC.OSCClient()
    cl.connect(addr)

    def send_message(url, *args):
        msg = OSC.OSCMessage()
        msg.setAddress(url)
        for l in args:
            msg.append(l)
        cl.send(msg)

    c = 1.25
    v = np.array([0, 0.4], dtype=float)
    b = 0.25
    lst = [[-12, -5, 0], [-8, -3, 4], [-5, -13, 2], [-3, 0, -8], [-2, -12, -8], [-3, 0, -8], [-5, -1, 2], [-8, -3, -20]]
    li = np.array(lst, dtype=int)

    def bld(l1, rep):
        for r in range(rep):
            for l in l1:
                for p in v/c:
                    dur = np.array([p, p+b], dtype=float)/c
                    send_message("/trigger/play", l, dur)
                    sleep(dur[1])

    li2 = [[li, 1], [li + 5, 1], [li, 2], [li + 5, 2], [li, 2], [li + 7, 1], [li + 5, 1], [li, 1], [li + 7, 1]]

    for l in li2:
        bld(l[0], l[1])
