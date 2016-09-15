.. title: Multisets and multivariate generating functions
.. slug: multisets-and-multivariate-generating-functions
.. date: 2015-07-03 17:00:24 UTC+05:30
.. tags: mathjax, multiset, generating function, simulation, J
.. category: 
.. link: 
.. description: 
.. type: text

Consider a multiset, :math:`S = \{11, 11, 11, 11, 12, 12, 12, 12, 13, 13, 13, 13, 13, 14, 14, 14, 14, 14, 14, 15, 15, 15\}`. 
*How many combinations of 8 elements can be made from the set so that the sum of those 8 elements is equal to 105, when the numbers are picked without replacement?*

Here, if the sum is not asked, the problem can be solved using an ordinary generating function as


.. math::

    G(x) = (1+x+x^2+x^3+x^4)^2\, (1+x+x^2+x^3+x^4+x^5)\, (1+x+x^2+x^3+x^4+x^5+x^6)\, (1+x+x^2+x^3)

and finding :math:`[x^8]G(x)`

But when the sum is also there as a constraint, we require one more variable to keep track of the sum. So, we may use :math:`x` to know the number of elements chosen, and :math:`y` for the sum of those numbers. Hence, the required bivariate generating function can be written as


.. math::

    G(x,y) = {\left(x^{6} y^{84} + x^{5} y^{70} + x^{4} y^{56} + x^{3} y^{42} + x^{2} y^{28} + x y^{14} + 1\right)}\\ {\left(x^{5} y^{65} + x^{4} y^{52} + x^{3} y^{39} + x^{2} y^{26} + x y^{13} + 1\right)} {\left(x^{4} y^{48} + x^{3} y^{36} + x^{2} y^{24} + x y^{12} + 1\right)}\\ {\left(x^{4} y^{44} + x^{3} y^{33} + x^{2} y^{22} + x y^{11} + 1\right)} {\left(x^{3} y^{45} + x^{2} y^{30} + x y^{15} + 1\right)}

Now, the answer to the question would be :math:`[x^8 y^{105}] G(x,y)` in the expansion of the product.

If the question was to find the number of combinations with replacement, the generating function can be represented as 


.. math::

    G(x,y) = \dfrac{1}{\left(1-x\, y^{11}\right)\left(1-x\, y^{12}\right)\left(1-x\, y^{13}\right)\left(1-x\, y^{14}\right)\left(1-x\, y^{15}\right)}

Now, let us focus our attention to a related probability problem:

*From the multiset S, what is the probability of choosing 8 elements such that the sum of those 8 elements is equal to 105, when the numbers are picked without replacement?*

This looks simple and we may be tempted to say that the answer is :math:`[x^8 y^{105}]G(x,y) / [x^8] G(x)`, but note that some combinations of numbers are more probable to be picked since the number of each element are not the same. E.g. If the set contains :math:`\{11, 11, 12\}`, the probability of choosing :math:`\{11, 11\}` will be more than the probability of choosing :math:`\{11, 12\}`.

Anyway, it can still be solved using a generating function: 


.. math::

    P(x,y) = \left(1+\binom{4}{1}\, (x\, y^{11})^{1}+\binom{4}{2}\, (x\, y^{11})^{2}+\binom{4}{3}\, (x\, y^{11})^{3}+\binom{4}{4}\, (x\, y^{11})^{4}\right)\\ \left(1+\binom{4}{1}\, (x\, y^{12})^{1}+\binom{4}{2}\, (x\, y^{12})^{2}+\binom{4}{3}\, (x\, y^{12})^{3}+\binom{4}{4}\, (x\, y^{12})^{4}\right)\\ \left(1+\binom{5}{1}\, (x\, y^{13})^{1}+\binom{5}{2}\, (x\, y^{13})^{2}+\binom{5}{3}\, (x\, y^{13})^{3}+\binom{5}{4}\, (x\, y^{13})^{4}+\binom{5}{5}\, (x\, y^{13})^{5}\right)\\ \left(1+\binom{6}{1}\, (x\, y^{14})^{1}+\binom{6}{2}\, (x\, y^{14})^{2}+\binom{6}{3}\, (x\, y^{14})^{3}+\binom{6}{4}\, (x\, y^{14})^{4}+\binom{6}{5}\, (x\, y^{14})^{5}+\binom{6}{6}\, (x\, y^{14})^{6}\right)\\ \left(1+\binom{3}{1}\, (x\, y^{15})^{1}+\binom{3}{2}\, (x\, y^{15})^{2}+\binom{3}{3}\, (x\, y^{15})^{3}\right)

and the required probability is 



.. math::

    \mathbb{P} = \dfrac{[x^8 y^{105}] P(x,y)}{\dbinom{22}{8}} = \dfrac{343}{2805} \approx 0.122281639928699

And what is the probability if we do it with replacement? In this case, the probability can be found by using an exponential generating function, which is written as: 

.. math::

    E(x,y) = e^{x\, \left(4\, y^{11}+4\, y^{12}+5\, y^{13}+6\, y^{14}+3\, y^{15} \right)}

and the probability is given by :math:`[x^8 y^{105}]E(x,y)\dfrac{8!}{22^8} = \dfrac{5621995920}{22^8} \approx 0.102449319851133`.

The above probabilities can also be verified by monte-carlo simulations in J, for the without replacement case:

.. code-block:: text
    :number-lines: 0

    lst=:(4#11,12),(5#13),(6#14),3#15
    sim=: 3 : '105=+/(8?#lst){lst'
    (+/%#)(sim"0)1e5#0

and for the with replacement case:

.. code-block:: text
    :number-lines: 0

    lst=:(4#11,12),(5#13),(6#14),3#15
    sim=: 3 : '105=+/(?8##lst){lst'
    (+/%#)(sim"0)1e5#0
