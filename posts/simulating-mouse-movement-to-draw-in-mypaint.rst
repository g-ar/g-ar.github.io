.. title: Simulating mouse movement to draw in mypaint
.. slug: simulating-mouse-movement-to-draw-in-mypaint
.. date: 2016-09-24 15:38:43 UTC+05:30
.. tags: mathjax, computer art, mypaint, xdotool, xorg, python
.. category: 
.. link: 
.. description: 
.. type: text

Wouldn't it be nice if we can command where to move the mouse pointer and press buttons, and then use the functionality to draw in a painting program? We can do exactly that in systems running ``xorg`` display server, by using ``xdotool`` to simulate mouse and keyboard.


E.g. to draw a simple 5 petal rose, :math:`r = \cos \left(5 \theta\right)`, 

.. code-block:: python
    :number-lines: 1

    from os import system
    from math import sin, cos, pi

    x, y = 1920, 1080
    xh, yh = x/2, y/2
    n = 1
    system('sleep 2; xdotool mousemove '+str(xh+yh)+' '+str(yh))

    for d in range(n*360+1):
        th = d*pi/180
        r = yh*cos(5*th)
        x, y = xh+r*cos(th), yh+r*sin(th)
        system('xdotool mousedown 1 mousemove '+str(x)+' '+str(y)+' mouseup 1') 
        system('sleep 0.01')

Below is the result of running the script with ``mypaint`` opened in fullscreen mode. Various effects can be achieved by selecting one among the hundreds of brushes available. 

.. figure:: ../../images/mypaint1.png

    Result of 08-micro felt pen and glow pen by running the script twice

Next is another example with a pencil style chosen for the brush.

.. code-block:: python
    :number-lines: 1

    from os import system
    from math import sin, cos, pi

    x, y = 800, 640
    xh, yh = x/2, y/2
    a,b,d = 2,3,61
    d = d*pi/180
    n = 2
    system('sleep 2; xdotool mousemove '+str(xh)+' '+str(yh))

    for i in range(n*360+1):
        x,y = xh+yh*sin(a*i*d)*sin(b*i*d), yh+yh*cos(a*i*d)*sin(b*i*d)
        system('xdotool mousedown 1 mousemove '+str(x)+' '+str(y)+' mouseup 1') 
        system('sleep 0.01')

.. figure:: ../../images/mypaint2.png

    Result of running the script with pencil

Interesting, yes?
