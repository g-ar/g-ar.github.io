.. title: Generating Music With SageMath And Sonic Pi - Examples - 2
.. slug: generating-music-with-sagemath-and-sonic-pi-examples-2
.. date: 2017-10-22 16:10:26 UTC+05:30
.. tags: python, sage, OSC, Sonic Pi, computer art
.. category: 
.. link: 
.. description: 
.. type: text

1 Blues Loop
------------

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

2 Morse Code
------------

Let's make a dynamic worksheet to generate morse code beeps.
Only alpha-numeric symbols are used here, and timings are taken from a `Sonic Pi example <https://github.com/rbnpi/SonicPi-Tutorials/blob/master/Morse.md>`_

In Sonic Pi,

.. code:: ruby
    :number-lines: 1

    live_loop :foo do
      use_synth :saw
      use_real_time
      t1, t2 = sync "/osc/trigger/morse"
      cue t1, t2
      play :C6, sustain: t1*0.9, release: t2*0.1
      sleep t1
    end

and in SageMath,

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

    codes = {
        'A': '.-',     'B': '-...',   'C': '-.-.', 
        'D': '-..',    'E': '.',      'F': '..-.',
        'G': '--.',    'H': '....',   'I': '..',
        'J': '.---',   'K': '-.-',    'L': '.-..',
        'M': '--',     'N': '-.',     'O': '---',
        'P': '.--.',   'Q': '--.-',   'R': '.-.',
        'S': '...',    'T': '-',      'U': '..-',
        'V': '...-',   'W': '.--',    'X': '-..-',
        'Y': '-.--',   'Z': '--..',   '0': '-----',
        '1': '.----',  '2': '..---',  '3': '...--',
        '4': '....-',  '5': '.....',  '6': '-....',
        '7': '--...',  '8': '---..',  '9': '----.' 
    }

    speed = 0.08
    code_timing = {'.': 1*speed, '-': 3*speed}
    element_gap = 1*speed
    char_gap = 3*speed
    word_gap = 7*speed - char_gap

    def to_morse_code(s):
        spl = s.split(' ')
        for l in spl:
            if l == '': continue # if multiple spaces are input
            mc = ' '.join(codes.get(i.upper()) for i in l)
            print l + ": " + mc

        for c in s:
            if c == ' ':
                sleep(word_gap)
                continue
            mc = codes.get(c.upper())
            for i in mc:
                send_message("/trigger/morse", code_timing[i], code_timing[i])
                sleep(element_gap + code_timing[i])
            sleep(char_gap + element_gap)


    @interact
    def _( msg=input_box(label='Enter Message', type=str, default='Hi'), auto_update=True):
        to_morse_code(msg)
