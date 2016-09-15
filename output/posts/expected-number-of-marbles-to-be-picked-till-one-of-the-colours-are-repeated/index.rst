.. title: Expected number of marbles to be picked till one of the colours are repeated
.. slug: expected-number-of-marbles-to-be-picked-till-one-of-the-colours-are-repeated
.. date: 2014-02-11 20:38:26 UTC+05:30
.. tags: mathjax, expected value, J, simulation, recurrence, sage
.. category: 
.. link: 
.. description: 
.. type: text

*There is a bag having 3 red, 3 black, 5 white and 7 green marbles. A marble is randomly picked one after another without replacement till the colour of the picked marble matches with one of the marbles in hand. What is the expected number of marbles we need to pick?*

Before trying out the analytical solution, let us get an approximate answer from a simulation.
It's just some tens of characters in J:

.. code-block:: text
    :number-lines: 0

    a=:(3#0 1),(5#2),(7#3)
    sim =: 3 : '{.1+I.-.~:(5?#a){a'
    (+/%#)(sim "0) 100000#0 NB. about 3.25279

sample size of 5 is chosen by pigeonhole principle.

- ``~:`` returns 1 for items which are distinct till that position.

- ``-.`` flips the 1's and 0's.

- ``I.`` fetches the indices of non-zero items. 1 added since indexing starts from 0.

- ``{.`` gets the head of the array.

For the analytical solution, it can be simply expressed as a recurrence relation:


.. math::

    \displaystyle f_{a,b,c,d} = \begin{cases} A+B+C+D-(a+b+c+d) & A-a = 2 \lor B-b = 2 \lor C-c = 2 \lor D-d = 2\\ & \\ \dfrac{1}{a+b+c+d}\left(a\cdot f_{a-1,b,c,d} + b\cdot f_{a,b-1,c,d} + c\cdot f_{a,b,c-1,d} + d\cdot f_{a,b,c,d-1}\right) & \text{otherwise} \end{cases}


where :math:`A,B,C,D` are the initial number of marbles of four colours.

And it can be directly translated to code.

In Sage:

.. code-block:: python
    :number-lines: 0

    A = 3
    B = 3
    C = 5
    D = 7
    def f(a,b,c,d):
        if (A-a == 2 or B-b == 2 or C-c == 2 or D-d == 2):
            return (A+B+C+D-(a+b+c+d))
        return (a*f(a-1,b,c,d) + b*f(a,b-1,c,d) + c*f(a,b,c-1,d) + d*f(a,b,c,d-1))/(a+b+c+d)
    ans= f(A,B,C,D)  
    print ans, N(ans) # = 3979/1224 and N() for the numerical approximation = 3.25081699346405

Here's an exercise for you to try:

Repeat the same problem, this time with the replacement of the marbles. What's the expected number of number of picks till you see the same coloured marble again?

Simulation is easy:

.. code-block:: text
    :number-lines: 0

    a=:(3#0 1),(5#2),(7#3)
    sim =: 3 : '{.1+I.-.~:(?5##a){a'
    (+/%#)(sim "0) 100000#0 

In a similar fashion, the recurrence can be modified. (Ans: :math:`757/243`)
