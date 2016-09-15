.. title: Generalized Derangements
.. slug: generalized-derangements
.. date: 2014-04-04 21:03:19 UTC+05:30
.. tags: mathjax, derangement, rook polynomial, integration, summation, sage
.. category: 
.. link: 
.. description: 
.. type: text

Derangement problems are quite easy to do when there are no restrictions.
Suppose we want to extend the problem with restrictions, e.g.:

*There are bins numbered 1,2,3,4,1,2,3,4,1,2 , and there are balls numbered 1,2,3,4,5,1,2,3,4,5 (distinguishable, say, two sets with different colors), and each bin is to contain a single ball such that the number of the bin and the ball should not match. In how many ways can that be done? (or what is the probability of that happening?)*

This can be solved by using Rook polynomials.

The rook polynomial for a :math:`m\times n` rectangle is:


.. math::

    \displaystyle r_{m,n}(x)=\sum_{k=0}^n{m\choose k}\, {n!\over (n-k)!}\, x^k

For this problem:

:math:`m=` number of bins numbered 's'

:math:`n=` number of balls numbered 's'

and the rook polynomial we require to solve our problem is:


.. math::

    \displaystyle R(x)&=r_{3,2}(x)\, r_{3,2}(x)\, r_{2,2}(x)\, r_{2,2}(x)\\\\ 
    R(x)&=(6\, x^2 + 6\, x + 1)^2\, (2\, x^2 + 4\, x + 1)^2

Now, with this rook polynomial, the number of ways of derangements can be found in two ways:


.. math::

    \displaystyle \int_0^\infty \, x^n\, R\left(-\dfrac{1}{x}\right)\, e^{-x}\, dx

where :math:`n` is the number of bins.

Another way is to expand :math:`R(x)` and replace each :math:`x^i` with :math:`i\cdot (n-i)!`

The answer divided by :math:`n!` gives the probability.

Both ways are described in the following Sage code:

.. code-block:: python
    :number-lines: 0

    var('k')
    def rp(m,n): return sum(binomial(SR(m),SR(k))*factorial(n)/factorial(n-k)*x^k,k,0,n)
    summ = 0
    balls = range(1,6)*2
    bins = ([1,2,3,4]*30)[:len(balls)]
    stbin = set(bins)
    nums = [bins.count(s) for s in stbin]
    rook(x) = expand(prod([rp(bins.count(s),balls.count(s)) for s in stbin]))
    nn = sum(nums)
    for i in range(nn+1):
        summ += (-1)^i*rook(x).coefficient(x,i)*factorial(nn-i)
    print summ/factorial(nn),"=",integrate(x^nn*rook(-1/x)*e^(-x),x,0,oo)/factorial(nn),"~",N(summ/factorial(nn)),summ

``283/2520 = 283/2520 ~ 0.112301587301587 407520``

which agrees with a simulation:

.. code-block:: text
    :number-lines: 0

    lst=.10$1 2 3 4
    a=.2#1+i.5
    sim=: 3 : '0=+/((10?10){a)=lst'
    (+/%#)(sim"0)1000000#0

which is about :math:`0.112101`

If we now turn our attention to the classic derangement problem, e.g. 10 bins and balls, each numbered 1 to 10, we change the variables accordingly:

.. code-block:: python
    :number-lines: 0

    balls = range(1,11) 
    bins = balls

We see that the summ is indeed :math:`1334961`, which is the number of derangements, :math:`D(10)`.

References:

`1. Stackexchange problem <https://math.stackexchange.com/questions/414023/probability-of-winning-the-game-1-2-3>`_

`2. Notes on rook polynomial <http://www.cs.uleth.ca/~holzmann/notes/rook.pdf>`_
