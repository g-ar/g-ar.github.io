.. title: Simulating Buffon's needle problem
.. slug: simulating-buffons-needle-problem
.. date: 2014-02-10 20:32:53 UTC+05:30
.. tags: mathjax, simulation, J
.. category: 
.. link: 
.. description: 
.. type: text

Buffon's needle problem is a classic problem on geometric probability. mathworld describes the problem quite well, and gives the following formula for the probability that the needle touches a line:


.. math::

    \displaystyle \mathbb{P}(x)=\begin{cases} \dfrac{2}{\pi}\, x& x\le 1\\ & \\ \dfrac{2}{\pi}\, \left(x-\sqrt{x^2-1}+\sec^{-1}{x}\right)& x>1 \end{cases}

How can we convince ourselves that the formula has been derived correctly? How to simulate geometric shapes?

The answer is to use the parametric representation of the points. If the needle of length :math:`l` is dropped, then it makes an angle :math:`\theta` with the horizontal. For simulating, take one end of the needle as a reference, and it must be randomly and uniformly distributed in :math:`[0,d)`. Call the random number as :math:`h`. The other end of the needle will then be at a distance :math:`h+l\cdot sin(\theta)`.

Now, for the condition that the needle touches the line, the other end must be either higher than :math:`d` or less than zero. Hence, we can simulate the experiment by picking :math:`h` in :math:`U(0,d)` and :math:`\theta \in U(0,2\, \theta)`.

Following is the code in J:

.. code-block:: text
    :number-lines: 1

    load 'trig'
    'l d'=:4 1    
        sim =: 3 : 0
    h=:d*?1000000#0
    th=:2p1*?1000000#0
    (+/%#)((h+l*sin th)>d)+.((h+l*sin th)<0)
    )
        sim 0

It gives an answer about :math:`0.919978`, and changing :math:`l` and :math:`d` to

.. code-block:: text
    :number-lines: 1

    'l d'=:1 4

gives about :math:`0.15907`, which are close to actual answers :math:`0.920000066713994` and :math:`0.159154943091895`, respectively.
