.. title: Context Free Grammars And Generating Functions
.. slug: context-free-grammars-and-generating-functions
.. date: 2018-01-27 14:43:04 UTC+05:30
.. tags: mathjax, context free grammar, generating function
.. category: 
.. link: 
.. description: 
.. type: text

We saw how to get the number of ways to generate a string of length n, matching or avoiding certain patterns, from Regular Expressions (RE).

But it can't help when more expressive power is required. We'll see examples on how to use Context Free Grammars (CFG) to obtain the generating functions (GF), whose coefficients in the formal power series indicate the number of parse trees possible for n-letter string for the given CFG.

So, in a way, it can be used to know whether the grammar is ambiguous or not.

Let's look at relatively simple examples, for the strings constructed over the set of symbols :math:`\left\{\left(, \right)\right\}`


.. raw:: html

    <font size="+1">1. Find the number of ways of constructing balanced parentheses</font>

The grammar is 


.. math::

    S \to (S)S\; \big| \; \epsilon

Then, to get its GF, replace each symbol with :math:`x`, and :math:`\epsilon` with 1. Hence, the GF for the RE is


.. math::

    S(x) &= x^2\, S(x)^2 + 1\\
    \implies S(x)&= \frac{1-\sqrt{1-4 x^2}}{2 x^2}\\
    &= \sum_{n\ge 0} \frac{1}{n+1}\binom{2\, n}{n} x^{2 n}

and the series is the well known Catalan numbers.


.. raw:: html

    <font size="+1">2. Find the number of ways of constructing balanced parentheses, which can have more opening parentheses</font>

e.g. for :math:`n=3`, :math:`(((, ((), ()(` are valid

We may obtain the CFG as


.. math::

    S \to (S)S \; \big|\; (S \; \big| \; \epsilon


Even though the grammar describes the language, it's actually ambiguous, and the GF obtained from this counts all the extra parse trees.

The right CFG is


.. math::

    S &\to B \;\big|\; U \\
      B &\to (\, B\, )\, B\; \big|\; \epsilon \\   
      U &\to (\,  S \;\big| \;(\, B\, )\, U \\


and we derive the GF


.. math::

    S(x) &= B(x) + U(x)\\
      B(x) &= B(x)^2 x^2  + 1\\
      U(x) &= S(x) x + B(x) U(x) x^2\\
      \implies S(x) &= \frac{-1+2\,x+\sqrt{1-4\,x^2}}{2\,x-4\,x^2}\\
      &= \sum_{n\ge 0} \binom{n}{\lfloor n/2 \rfloor} x^{n}

Read `Gruber, Lee & Shallit <https://arxiv.org/abs/1204.4982>`_ for the theory.
