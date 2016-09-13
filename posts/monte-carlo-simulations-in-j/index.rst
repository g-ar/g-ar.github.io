.. title: Monte-Carlo simulations in J
.. slug: monte-carlo-simulations-in-j
.. date: 2014-03-29 12:16:24 UTC+05:30
.. tags: mathjax
.. category: 
.. link: 
.. description: 
.. type: text

We will see some more problems on probability, and how to do it in J.

1 Derangement problem
---------------------

*There are 30 boxes numbered 1 to 30, and 30 balls numbered 1 to 30. If the balls are put into the boxes (one in each) randomly, what's the probability that none of the balls are put in the boxes with matching number?*

By simulation:

.. code-block:: text
    :number-lines: 0

    sim=: 3 : '0=+/(i.30)=30?30'
    (+/%#)(sim"0)100000#0

The analytical answer:

.. code-block:: text
    :number-lines: 0

    +/((30$(1 _1))*(%!i.30))

2 Urns, balls and a gamble.
---------------------------

A game is played with the following rules: There is an urn with 20 balls, 10 red and 10 white. You need pick 10 balls out of these 20.

1. If 10 balls are of the same color, you get $300

2. If 9 balls are of the same color, you get $30

3. If 8 balls are of the same color, you get $3

4. If 7 balls are of the same color, you get $2

5. If 6 balls are of the same color, you get $1

6. If 5 balls are of the same color, you lose $5

What's your expected amount?

Simulation answer:

.. code-block:: text
    :number-lines: 0

    a=: 10#0 1
    sim=: 3 : '((_5 * 5 = ]) + ([: +/ 4 6 =/ ]) + (2 * [: +/ 3 7 =/ ]) + (3 * [: +/ 2 8 =/ ]) + (30 * [: +/ 1 9 =/ ]) + 300 * [: +/ 10 0 =/ ])+/(10?20){a'
    (+/%#)(sim"0)1000000#0  NB. = _0.826702

Analytical answer (hypergeometric distribution):

:math:`A=\{1, 2, 3, 30, 300, -5\}`



.. math::

    \displaystyle \sum_{i=0}^{4}\dfrac{A_i\cdot 2\, \dbinom{10}{i}\dbinom{10}{10-i}}{\dbinom{20}{10}}+\dfrac{A_5\cdot \dbinom{10}{5}^2}{\dbinom{20}{10}} =-\dfrac{76485}{92378}=-0.827956872848514

.. code-block:: text
    :number-lines: 0

    _5 1 2 3 30 300)*(((5!10)^2), (2 * (10 !~ ]) * 10 !~ 10 - ]) 6+i.5)%10!20

3 Birthday problem
------------------

How many people should be in a room such that the probability of at least two of them sharing the same birthday is more than 0.5? (Assume 365 days in an year)

By Simulation:

.. code-block:: text
    :number-lines: 0

    sim=: 3 : '23>+/~:?23#365'
    (+/%#)(sim"0)100000#0

Analytically:

.. code-block:: text
    :number-lines: 0

    1-*/((365-i.23)%(365)) NB. = 0.507297234323985

To see a plot of the probabilities, up to 100 people in a room:

.. code-block:: text
    :number-lines: 0

    load'plot'
    'marker' plot (1+i.100);((1 - [: */ 365 %~ 365 - i.)"0) 1+i.100

4 4 dice
--------

Four dice are rolled, what's the probability that no two faces are repeated? (E.g. 6 4 2 5 is okay, but 3 6 5 6 is forbidden)

By simulation:

.. code-block:: text
    :number-lines: 0

    sim=: 3 : '(4 = [: +/ [: +/ =/~)?4#6'

By permutation:


.. math::

    \displaystyle \mathbb{P}=\dfrac{\dbinom{6}{4}\cdot 4!}{6^4} = \dfrac{5}{18} = 0.2777777


Putting that in J console:

.. code-block:: text
    :number-lines: 0

    ((4!6)*!4)%6^4

By counting:

.. code-block:: text
    :number-lines: 0

    (+/4=+/"1~:"1(6 6 6 6#:i.1296))%6^4 NB. this uses base-6 representation till 6^4

5 The ballot problem
--------------------

In an election, candidate A receives n votes, and candidate B receives m votes where :math:`n > m`. Assuming that all orderings are equally likely, show that the probability that A is always ahead in the count of votes is :math:`\dfrac{n - m}{n + m}`.

Here's a simulation to see that it may be true:

.. code-block:: text
    :number-lines: 0

    n=:55
    m=:45
    a=:(m#_1),n#1
    sim=: 3 : '100=+/0<+/\(100?100){a'
    (+/%#)(sim"0)100000#0 NB. = 0.0993

which is close to 0.1 computed from the formula.

6 An ace from a regular deck of 52 cards
----------------------------------------

A deck of cards is well shuffled, and placed face down on a table. The cards are picked one after another, what's the probability that you get to see the first ace when :math:`k^{th}` card is picked?

Finding the answer is quite easy, which is



.. math::

    \displaystyle \mathbb{P}(k)=\dfrac{\dbinom{48}{k-1}}{\dbinom{52}{k-1}}\cdot \dfrac{4}{52-k}

and a simulation can back up our claim, e.g. for :math:`k=5`:

.. code-block:: text
    :number-lines: 0

    pos=: 5
    a=:(4#1),48#0
    sim=: 3 : 'pos=1+{.I.(52?52){a'
    (+/%#)(sim"0)100000#0

and the probabilities for first five positions:

.. code-block:: text
    :number-lines: 0

    (((48 !~ 1 -~ ]) % 52 !~ 1 -~ ]) * 4 % 52 - 1 -~ ]) 1+i.5
