.. title: Applications of Generating Functions
.. slug: applications-of-generating-functions
.. date: 2014-03-19 20:54:04 UTC+05:30
.. tags: mathjax, generating function, sage, diophantine equations, summation, expected value, probability, infinite series
.. category: 
.. link: 
.. description: 
.. type: text

Generating Function (g.f.) is one of the most important tools in combinatorics. Many problems which are difficult to do by combinatorial arguments and by cases are trivial when g.f. is applied. Below are some typical problems where g.f. are very useful.

1 Solving diophantine equations (d.e.)
--------------------------------------

a) What are the number of solutions for the linear d.e.


.. math::

    \displaystyle x_1+x_2+x_3+x_4=35\\ 1\le x_1,x_2\le 20\\ 20\le x_3,x_4 \le 30


Answer can be easily found by representing the variables as polynomials and multiplying them, i.e.


.. math::

    \displaystyle [z^{35}](z+z^2+z^3+\cdots +z^{20})^2\, (z^{20}+z^{21}+z^{22}+\cdots +z^{30})^2 = 11


Hence, there are 11 solutions for the d.e under the constraints.

In Sage, this can be found by the taylor expansion:

.. code-block:: python
    :number-lines: 0

    taylor((x-x^21)/(1-x)*(x^20-x^31)/(1-x),x,0,35).coefficient(x,35)

We can even get a closed form if the problem is changed to


.. math::

    \displaystyle x_1+x_2+x_3+x_4=n\\ 0\le x_1,x_2, x_3,x_4


The g.f. for this is written as


.. math::

    \displaystyle (1+z+z^2+z^3+\cdots)^4=\dfrac{1}{(1-z)^4}


and the nth coeffiecient is


.. math::

    \displaystyle [z^n]\dfrac{1}{(1-z)^4}=\dbinom{n+3}{3}

b) Number of ways to make changes for n units using coins of denominations 1,2 and 5 units.
The d.e. for this problem is written as


.. math::

    \displaystyle x_1+2\, x_2+5\, x_3=n


and the g.f. is


.. math::

    \displaystyle G(z)=\dfrac{1}{(1-z)(1-z^2)(1-z^5)}


The :math:`n^{th}` coefficient gets the exact answer, but having a g.f. allows us to do more, like having an asymptotic form.
In this case, factor the denominator


.. math::

    \displaystyle G(z)=\dfrac{1}{(1-z)^3}\cdot \dfrac{1}{(1+z)(1+z+z^2+z^3+z^4)}


The first part of the two fractions contributes the most to the coefficients, and an approximation can be written as


.. math::

    \displaystyle [z^n]G(z)\sim [z^n]\dfrac{1}{(1-z)^3}\cdot \lim_{z\to 1}\dfrac{1}{(1+z)(1+z+z^2+z^3+z^4)} =\dfrac{\dbinom{n+2}{2}}{10}


Comparing the precise and approximate answers:
for :math:`n=1000`
:math:`50401` and :math:`50150.1`,

for :math:`n=2000`
200801 and 200301.1,
respectively.

2 Partial and infinite sums
---------------------------

a) Finding a closed form for


.. math::

    \displaystyle \sum_{k=1}^n k


For problems like this, we can make use of the property of g.f:


.. math::

    \displaystyle G(z)=\sum_{i\ge 0} a_i\, x^i\\ \implies \dfrac{1}{1-z}G(z)=\sum_{i\ge 0}\left(\sum_{j=0}^i a_j \right)z^i


G.f. for :math:`<0,1,2,3,\ldots>` is given by


.. math::

    \displaystyle G(z)=\dfrac{z}{(1-z)^2}


Hence, to find the sum, it's simply


.. math::

    \displaystyle \dfrac{1}{1-z}\, G(z)=\dfrac{z}{(1-z)^3}\\ \implies [z^n]\dfrac{1}{1-z}\, G(z)=\sum_{k=1}^n k = \dbinom{n+1}{2}


We can then proceed to find more complicated sums like


.. math::

    \displaystyle \sum_{k=1}^n k^2


which are usually done in high school by mathematical induction, but never shown how to derive in the first place.
To derive it from g.f., we must first find the g.f. for , which can be easily obtained by differentation. i.e.


.. math::

    \displaystyle z\, \dfrac{d}{dz}\left(\frac{z}{(1-z)^2}\right)=\dfrac{z+z^2}{(1-z)^3}


and the sum of the squares is


.. math::

    \displaystyle [z^n]\dfrac{z+z^2}{(1-z)^4}=\dbinom{n+2}{3}+\dbinom{n+1}{3}=\dfrac{1}{6} \, {\left(2 \, n + 1\right)} {\left(n + 1\right)} n


which can be verified by Sage

.. code-block:: python
    :number-lines: 0

    var('n');
    show(factor(sum(x^2,x,0,n)))

b) Summation of infinite series

For this purpose, the dummy variable in the g.f. is replaced by a constant to obtain the closed form. E.g.
i)


.. math::

    \displaystyle \sum_{i\ge 0}\frac{1}{5^i}


We have the g.f. for :math:`< 1,1,1,\ldots >`


.. math::

    \displaystyle G(z)=\dfrac{1}{1-z}\\ \implies G(1/5)=\dfrac{5}{4}


ii) We can also solve seemingly complicated sums like


.. math::

    \displaystyle \sum_{i\ge 0} \dfrac{i^2\, F_i}{12^i}


where :math:`F_i` is the :math:`i^{th}` fibonacci term.

The g.f. for :math:`F_i` can be shown to be (derived from the linear recurrence relation)


.. math::

    \displaystyle G(z)=\dfrac{z}{1-z-z^2}


Next, obtain the g.f. for :math:`i^2 F_i`


.. math::

    \displaystyle G_1(z)= z\, D\left(z\, D\left( \dfrac{z}{1-z-z^2} \right) \right) = \dfrac{z^5 - z^4 + 6\, z^3 + z^2 + z}{(1-z-z^2)^3}


where :math:`D` is the differential operator :math:`d/dz` and


.. math::

    \displaystyle \sum_{i\ge 0} \dfrac{i^2\, F_i}{12^i} = G_1(1/12) = \dfrac{279804}{2248091} \approx 0.124462933217561


In Sage, it's written as

.. code-block:: python
    :number-lines: 0

    (x*diff(x*diff(x/(1-x-x^2),x),x)).subs(x=1/12)

3 As probability g.f. and obtain expected values
------------------------------------------------

In probability g.f., the coefficients are the probability of occurance of the value. The p.g.f. for a die with 6 faces numbered 1 to 6 is given by


.. math::

    \displaystyle P(x)=\dfrac{x+x^2+x^3+x^4+x^5+x^6}{6}


and the probability of the sums when two dice are thrown


.. math::

    \displaystyle P_2(x)=P(x)^2=\frac{1}{36} \, x^{12} + \frac{1}{18} \, x^{11} + \frac{1}{12} \, x^{10} + \frac{1}{9} \, x^{9} + \frac{5}{36} \, x^{8} + \frac{1}{6} \, x^{7} + \frac{5}{36} \, x^{6} + \frac{1}{9} \, x^{5} + \frac{1}{12} \, x^{4} + \frac{1}{18} \, x^{3} + \frac{1}{36} \, x^{2}


and the expected value shown when the die is thrown:


.. math::

    \displaystyle E_1=P'(1)=\dfrac{7}{2}


and when two dice are thrown:


.. math::

    \displaystyle E_2=P_2'(1)=\dfrac{7\cdot 2}{2}
