.. title: Regular Expressions And Generating Functions
.. slug: regular-expressions-and-generating-functions
.. date: 2018-01-26 15:37:34 UTC+05:30
.. tags: mathjax, regular expression, generating function
.. category: 
.. link: 
.. description: 
.. type: text

We'll discuss about the number of ways to generate a string of length n, matching or avoiding certain patterns as its substring.

This can be easily achieved deriving an unambiguous regular expression (RE) by constructing a minimal Deterministic Finite Automaton (DFA), and then the ordinary Generating Function (GF) whose coefficients are the number of n-letter strings having the pattern.

Let's look at a few examples, for the strings constructed over the set of symbols :math:`\{0, 1\}` such that


.. raw:: html

    <font size="+1">1. There are no two consecutive 1's</font>

The RE is: :math:`(0+10)^*(1+\epsilon)`

Then, to get its GF, replace each symbol with :math:`x`, and :math:`\epsilon` with 1. Hence, the GF for the RE is


.. math::

    G_1(x) &= \frac{1}{1-x-x^2}\cdot (1+x)\\
    &= \sum_{n\ge 0} \frac{ \left(\left(\sqrt{5}-3\right)
       \left(1-\sqrt{5}\right)^n+\left(\sqrt{5}+3\right)
       \left(1+\sqrt{5}\right)^n\right)}{\sqrt{5}\, 2^{n+1}} x^n\\
    &= \sum_{n\ge 0} F_{n+2} x^n

where :math:`F_{n}` is the fibonacci number


.. raw:: html

    <font size="+1">2. At least one pair of consecutive 1's as its substring</font>

RE: :math:`(0+10)^*11(0+1)^*`



.. math::

    G_2(x) &= \frac{1}{1-x-x^2} \cdot x^2\cdot \frac{1}{1-2\, x}\\
    &= \sum_{n\ge 0} \frac{ \left(\left(\sqrt{5}-3\right)
       \left(-\left(1-\sqrt{5}\right)^n\right)-\left(3+\sqrt{5}\right)
       \left(1+\sqrt{5}\right)^n\right)}{\sqrt{5}\, 2^{n+1}}+2^n\\
    &= \sum_{n\ge 0}\left(2^{n} - F_{n+2}\right) x^n

which can also be confirmed from the first example by taking its complement, since the number of possible words are :math:`2^n`


.. raw:: html

    <font size="+1">3. At least one 01 as its substring</font>

RE: :math:`1^*00^*1(0+1)^*`




.. math::

    G_3(x) &= \frac{x^2}{(1-2 x) (1-x)^2}\\
    &= \sum_{n\ge 0} \left(2^n-n-1\right) x^n

For more, refer `Analytic Combinatorics <http://algo.inria.fr/flajolet/Publications/book.pdf>`_, which is a treatise on Generating Functions.
