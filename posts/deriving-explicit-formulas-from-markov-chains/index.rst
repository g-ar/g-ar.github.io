.. title: Deriving Explicit Formulas from Markov Chains
.. slug: deriving-explicit-formulas-from-markov-chains
.. date: 2015-08-09 17:11:29 UTC+05:30
.. tags: mathjax, generating function, sage, markov chain, J, simulation
.. category: 
.. link: 
.. description: 
.. type: text

Once we formulate the markov model correctly, we can obtain the generating function for each entry in the matrix, where there's a possibility of getting the explicit formula. Let's take a look at one such problem:

*A six faced unbiased die is rolled :math:`n` times. What is the probability that we get to see all the six numbers in the sequence?*

Setting up a markov chain is easy:


.. math::

    \displaystyle A= \begin{pmatrix} 0 & 1 & 0 & 0 & 0 & 0 & 0\\ 0 & \frac{1}{6} & \frac{5}{6} & 0 & 0 & 0 & 0\\ 0 & 0 & \frac{1}{3} & \frac{2}{3} & 0 & 0 & 0\\ 0 & 0 & 0 & \frac{1}{2} & \frac{1}{2} & 0 & 0\\ 0 & 0 & 0 & 0 & \frac{2}{3} & \frac{1}{3} & 0\\ 0 & 0 & 0 & 0 & 0 & \frac{5}{6} & \frac{1}{6}\\ 0 & 0 & 0 & 0 & 0 & 0 & 1 \end{pmatrix}


The states indicate the number of faces shown up. E.g. the row above the last row indicates that when 5 faces are seen, there's a probability of :math:`5/6` remaining in the same state and :math:`1/6` moving to the final state.

So, :math:`A^n[0,6]`, gives the required answer. But we can also find the generating function for that entry by computing :math:`(I-x\, A)^{-1}`, which is


.. math::

    \displaystyle G(x) = \frac{10 \, x^{6}}{{\left(5 \, x - 6\right)} {\left(2 \, x - 3\right)} {\left(x - 1\right)} {\left(x - 2\right)} {\left(x - 3\right)} {\left(x - 6\right)}}


and on partial fractions it's


.. math::

    \displaystyle G(x) = \frac{36}{5 \, x - 6} - \frac{45}{2 \, x - 3} - \frac{1}{x - 1} + \frac{40}{x - 2} - \frac{45}{x - 3} + \frac{36}{x - 6} + 1


and the probability can be written by extracting :math:`[x^n]G(x)` as


.. math::

    \displaystyle \mathbb{P}(n) = 1-\frac{6}{6^n}+\frac{15}{3^n}-\frac{20}{2^n}+15\left(\frac{2}{3}\right)^n-6\left(\frac{5}{6}\right)^n


which can be verified by a simulation in J:

.. code-block:: text
    :number-lines: 0

    n=:10
    sim=: 3 : '6=+/~:?n#6'
    (+/%#)(sim"0)1e5#0 NB. about 0.27

and


.. math::

    \mathbb{P}(10) = \dfrac{38045}{139968} \approx 0.271812128486511
