.. title: Probability of a sum being less than 1 (convolution of pdf)
.. slug: probability-of-a-sum-being-less-than-1-convolution-of-pdf
.. date: 2014-06-01 15:23:45 UTC+05:30
.. tags: mathjax
.. category: 
.. link: 
.. description: 
.. type: text

If we choose :math:`n` numbers randomly and uniformly from :math:`[0,1]` and raise each number to :math:`k` :math:`(k>0)`, what is the probability that the sum will be less than 1?

i.e. what is


.. math::

    \displaystyle \mathbb{P}\left(\Big(\sum_{i=1}^n x_i^k\Big)<1\right)\;\;\; , x_i\in \mathcal{U}\left(0,1\right)

The answer can be derived by using convolution of probability density functions.

For two pdfs, :math:`f(x)` and :math:`g(y)`, the convolution :math:`f*g` is defined as


.. math::

    \displaystyle \left(f*g\right)(z) &= \int_{-\infty}^{\infty} \, f(z-y)g(y)\, dy\\ 
    &= \int_{-\infty}^{\infty} \, g(z-x) f(x)\, dx

Let :math:`X_1` and :math:`X_2` be two random variables which represent :math:`x_1^{1/k}` and :math:`x_2^{1/k}`. 

The pdf is then given by:


.. math::

    \displaystyle f_{X_1}(x)=f_{X_2}(x)=\frac{d}{dx} (x^k)=k\, x^{k-1}

and the pdf of the sum of two numbers :math:`f_2(z)` (in the region :math:`0\le z \le 1`) is:


.. math::

    \displaystyle f_2(z)&= \int_{0}^{z} \, f_{X_1}(z-y) f_{X_2}(y) \, dy\\ 
    &= k^2\cdot z^{2\, k - 1}\cdot \mathrm{B}(k, k)

After this, we can iteratively continue for more terms:


.. math::

    \displaystyle f_3(z)&= \int_{0}^{z} \, f_{X_1}(z-y) f_{2}(y) \, dy\\ 
    &= k^3\cdot z^{3\, k - 1}\cdot \mathrm{B}(2\, k, k)\cdot \mathrm{B}(k, k)

Continuing in that manner, for sum of :math:`n` terms, we will end up with:


.. math::

    \displaystyle f_n(z) = k^n\cdot z^{n\, k - 1}\cdot \prod_{i=1}^{n-1} \mathrm{B}(i\, k, k)

Since we require the probability of the sum to be less than one, we will evaluate that integral and write the beta functions in terms of gamma functions and simplify:


.. math::

    \displaystyle \mathbb{P}\left(\Big(\sum_{i=1}^n x_i^k\Big)<1\right) &= \int_{0}^{1} \, f_n(z)\, dz\\ 
    &= \frac{k^{n-1}}{n}\prod_{i=1}^{n-1} \mathrm{B}(i\, k, k) \\ 
    &= \frac{k^{n-1}\big(\Gamma(k)\big)^n}{n\, \Gamma(n\, k)} \\ 
    &= \frac{\big(\Gamma(k+1)\big)^n}{\Gamma(n\, k+1)}

That's some formula!

In Sage, it can be written as

.. code-block:: python
    :number-lines: 0

    var('k y z')
    assume(k>0)
    assume(2*k-1>0)
    f1(z) = diff(z^k,z)
    f2(z) = f(z)
    for i in range(10):
        f2(z) = integrate(f2(z-y)*f1(y),y,0,z)
        print i+2, f2(z)

    f(n,k) = gamma(k+1)^n/gamma(n*k+1)

To verify our answer, we can perform a simulation:

.. code-block:: text
    :number-lines: 0

    sim=: 3 : '1>+/(?6#0)^6'
    (+/%#)(sim"0)100000#0 NB. = 0.63926

If we want to perform a simulation in a more verbose language, R is a good candidate.

The code in R looks like:

.. code-block:: R
    :number-lines: 0

    n = 6
    k = 6
    b = array(runif(n*1e6,0,1),dim=c(n,1e6))
    mean(apply(b^k,2,sum)<1)

and if we want to perform using two dimensional arrays in J also, the equivalent code can be written as:

.. code-block:: text
    :number-lines: 0

    'n k'=: 6 6
    a=:(n, 1e6) $ ?(n*1e6)#0
    (+/%#)1>+/a^k NB. = 0.637572

The corresponding probability derived analytically is:


.. math::

    \displaystyle \mathbb{P}\left(\Big(\sum_{i=1}^6 x_i^6\Big)<1\right) = f\left(6,\frac{1}{6}\right) \\ = \Gamma\left(\frac{7}{6}\right)^6 \approx 0.637528558759471

References:

`1. Dartmouth Probability Book <http://www.dartmouth.edu/~chance/teaching_aids/books_articles/probability_book/Chapter7.pdf>`_

`2. An arxiv article <http://arxiv.org/pdf/cs/0604056.pdf>`_

`3. Volume of a n-ball <https://en.wikipedia.org/wiki/Volume_of_an_n-ball>`_
