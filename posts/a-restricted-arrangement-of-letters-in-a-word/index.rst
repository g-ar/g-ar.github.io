.. title: A restricted arrangement of letters in a word
.. slug: a-restricted-arrangement-of-letters-in-a-word
.. date: 2014-04-05 13:25:01 UTC+05:30
.. tags: mathjax
.. category: 
.. link: 
.. description: 
.. type: text

*Find number of ways to arrange letters in word "benzine" such that no two same letters are adjacent to each other*

This is quite a tough nut to crack. But, there are multiple ways to solve this: by summation, generating function, a recurrence and by integration; summation being the most efficient one.

1 By summation
--------------

We'll start with a generalized formula for 5 distinct objects, each with :math:`n_i` repetitions. The summation formula can be written as:


.. math::

    \displaystyle N(n_1,n_2,n_3,n_4,n_5)=\sum_{x_{1}=1}^{n_{1}}\sum_{x_{2}=1}^{n_{2}}\sum_{x_{3}=1}^{n_{3}}\sum_{x_{4}=1}^{n_{4}}\sum_{x_{5}=1}^{n_{5}}\, (-1)^{n_{1}+n_{2}+n_{3}+n_{4}+n_{5}-x_{1}-x_{2}-x_{3}-x_{4}-x_{5}}\, \dfrac{(x_{1}+x_{2}+x_{3}+x_{4}+x_{5})!}{x_1!x_2!x_3!x_4!x_5!} \cdot \dbinom{n_{1}-1}{x_{1}-1}\,\dbinom{n_{2}-1}{x_{2}-1}\,\dbinom{n_{3}-1}{x_{3}-1}\,\dbinom{n_{4}-1}{x_{4}-1}\,\dbinom{n_{5}-1}{x_{5}-1}

and for our problem, we need to find :math:`N(2,2,1,1,1)`. Putting that in Sage:

.. code-block:: python
    :number-lines: 0

    var('x1 x2 x3 x4 x5')
    n1,n2,n3,n4,n5 = 2,2,1,1,1
    sum(sum(sum(sum(sum((-1)^(n1+n2+n3+n4+n5-x1-x2-x3-x4-x5)*factorial(x1+x2+x3+x4+x5)/(factorial(x1)*factorial(x2)*factorial(x3)*factorial(x4)*factorial(x5))*binomial(SR(n1)-1,x1-1)*binomial(SR(n2)-1,x2-1)*binomial(SR(n3)-1,x3-1)*binomial(SR(n4)-1,x4-1)*binomial(SR(n5)-1,x5-1),x1,1,n1),x2,1,n2),x3,1,n3),x4,1,n4),x5,1,n5)

:math:`=660`

2 Generating function
---------------------

This is actually just a part of the generating function (which I think can be derived from the recurrence relation)



.. math::

    \displaystyle G_n(v,w,x,y,z)=\left(\begin{array}{rrrrr} v & w & x & y & z \end{array}\right)\cdot \left(\begin{array}{rrrrr} 0 & w & x & y & z \\ v & 0 & x & y & z \\ v & w & 0 & y & z \\ v & w & x & 0 & z \\ v & w & x & y & 0 \end{array}\right)^{n-1}\cdot \left(\begin{array}{r} 1 \\ 1 \\ 1 \\ 1 \\ 1 \end{array}\right)

The coefficient of :math:`v^2 w^2 x y z` is the answer (evidently 660).

In Sage:

.. code-block:: python
    :number-lines: 0

    var('v w x y z')
    a = matrix([v, w, x, y, z])
    A = matrix([[0, w, x, y, z], [v, 0, x, y, z], [v, w, 0, y, z], [v, w, x, 0, z], [v, w, x, y, 0]])
    ones = ones_matrix(QQ,5,1)
    fxt = expand(a*(A^6)*ones)[0,0]
    fxt.coefficient(x,2).coefficient(y,2).coefficient(z,1).coefficient(v,1).coefficient(w,1)

3 By recurrence
---------------



.. math::

    \displaystyle f_{p,q,r,s,t}=\left\{\begin{matrix} 0 & \text{if any of }p,q,r,s,t<0\\ 1 & \text{if any one of }p,q,r,s,t = 1 \text{ and others } 0\\ 2 & \text{if any two of }p,q,r,s,t = 1 \text{ and others } 0\\ 6 & \text{if any three of }p,q,r,s,t = 1 \text{ and the other's } 0\\ 24 & \text{if any four of }p,q,r,s,t = 1 \text{ and the other } 0\\ 120 & \text{if all of }p,q,r,s,t = 1 \\ A & \text{otherwise} \end{matrix}\right.

where


.. math::

    \displaystyle A=f_{p-1,q-1,r,s,t} + f_{p-1,q,r-1,s,t} + f_{p-1,q,r,s-1,t} + f_{p-1,q,r,s,t-1} + f_{p,q-1,r-1,s,t}+\\ f_{p,q-1,r,s-1,t} + f_{p,q-1,r,s,t-1} + f_{p,q,r-1,s-1,t} + f_{p,q,r-1,s,t-1} + f_{p,q,r,s-1,t-1}+\\ 2\cdot (f_{p-1,q-1,r-1,s,t} + f_{p-1,q-1,r,s-1,t} + f_{p-1,q-1,r,s,t-1} + f_{p-1,q,r-1,s-1,t}+ f_{p-1,q,r-1,s,t-1}+\\ f_{p-1,q,r,s-1,t-1} + f_{p,q-1,r-1,s-1,t} + f_{p,q-1,r-1,s,t-1} + f_{p,q-1,r,s-1,t-1} + f_{p,q,r-1,s-1,t-1})+\\ 3\cdot (f_{p-1,q-1,r-1,s-1,t} + f_{p-1,q-1,r-1,s,t-1} + f_{p-1,q-1,r,s-1,t-1} + f_{p-1,q,r-1,s-1,t-1} + f_{p,q-1,r-1,s-1,t-1})+4\cdot f_{p-1,q-1,r-1,s-1,t-1}

In Sage (python will also do), to print out the number of arrangements and its probability of occuring:

.. code-block:: python
    :number-lines: 0

    import numpy as np
    def f(p,q,r,s,t) :
        if sum([0]>np.array([p,q,r,s,t]))>0:
            return 0
        if sum([1]==np.array([p,q,r,s,t]))==5:
            return 120
        if sum([1]==np.array([p,q,r,s,t]))==4 and sum([0]==np.array([p,q,r,s,t]))==1:
            return 24
        if sum([1]==np.array([p,q,r,s,t]))==3 and sum([0]==np.array([p,q,r,s,t]))==2:
            return 6
        if sum([1]==np.array([p,q,r,s,t]))==2 and sum([0]==np.array([p,q,r,s,t]))==3:
            return 2
        if sum([1]==np.array([p,q,r,s,t]))==1 and sum([0]==np.array([p,q,r,s,t]))==4:
            return 1     
        return f(p-1,q-1,r,s,t)+ f(p-1,q,r-1,s,t)+ f(p-1,q,r,s-1,t)+ f(p-1,q,r,s,t-1)+ f(p,q-1,r-1,s,t)+ f(p,q-1,r,s-1,t)+ f(p,q-1,r,s,t-1)+ f(p,q,r-1,s-1,t)+ f(p,q,r-1,s,t-1)+ f(p,q,r,s-1,t-1)+2*(f(p-1,q-1,r-1,s,t)+ f(p-1,q-1,r,s-1,t)+ f(p-1,q-1,r,s,t-1)+ f(p-1,q,r-1,s-1,t)+ f(p-1,q,r-1,s,t-1)+ f(p-1,q,r,s-1,t-1)+ f(p,q-1,r-1,s-1,t)+ f(p,q-1,r-1,s,t-1)+ f(p,q-1,r,s-1,t-1)+ f(p,q,r-1,s-1,t-1)) +3*(f(p-1,q-1,r-1,s-1,t)+ f(p-1,q-1,r-1,s,t-1)+ f(p-1,q-1,r,s-1,t-1)+ f(p-1,q,r-1,s-1,t-1)+ f(p,q-1,r-1,s-1,t-1))+4*f(p-1,q-1,r-1,s-1,t-1)
    a1,a2,a3,a4,a5 = 2,2,1,1,1
    tot = f(a1,a2,a3,a4,a5)
    print tot, tot/(factorial(a1+a2+a3+a4+a5)/(factorial(a1)*factorial(a2)*factorial(a3)*factorial(a4)*factorial(a5)))

NumPy is used here, since it allows array to be manipulated just like in an array programming language like J. The condition checking is made much shorter.

The recurrence is too slow if used for higher values. This can be sped up by caching the computed values, e.g. by dynamic programming.

We may back up our analytical results with a simulation (always a good thing to do, when possible)

.. code-block:: text
    :number-lines: 0

    a=.1 1 2 2 3 4 5
    sim=: 3 : '0=+/0=2-/\(7?7){a'
    (+/%#)(sim"0)1000000#0

about :math:`0.523729`, which is close to the actual result :math:`0.523809523809524`.

4 Using Integrals
-----------------

One more way is to make use of integrals, which actually conveys the summation in a compact representation.



.. math::

    \displaystyle N(\{n_i\})=\int_0^\infty \prod_i q_{n_i}(x)\, e^{-x} \, dx

where



.. math::

    \displaystyle q_{n_i}(x) = \sum_{i=1}^{n_i} \frac{(-1)^{i-n_i}}{i!} {n_i-1 \choose i-1}x^i \text{ for }n_i\geq 1

:math:`n_i` is the number of repetitions of each character in the set.

For our example, the list of :math:`n_i` can be written as [2,2,1,1,1]

Hence, in Sage:

.. code-block:: python
    :number-lines: 0

    var('i')
    def q(n): return sum((-1)^(i-n)/factorial(i)*binomial(SR(n)-1,SR(i)-1)*x^i, i, 1, n)
    lst = [2,2,1,1,1]
    integrate(prod([q(l) for l in lst])*e^-x, x, 0, oo)

which displays our expected answer.

References:

`1. Brilliant.org discussion <https://brilliant.org/discussions/thread/permutation-problemneed-some-experts/>`_

`2. Stackexchange question <https://math.stackexchange.com/questions/76213/how-many-arrangements-of-a-2b-3c-4d-5e-have-no-identical-consecutive-lett>`_

`3. Another stackexchange question <https://math.stackexchange.com/questions/129451/find-the-number-of-arrangements-of-k-mbox-1s-k-mbox-2s-cdots>`_
