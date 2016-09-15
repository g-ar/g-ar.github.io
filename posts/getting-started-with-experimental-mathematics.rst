.. title: Getting started with experimental mathematics
.. slug: getting-started-with-experimental-mathematics
.. date: 2014-12-16 16:47:39 UTC+05:30
.. tags: mathjax, expected value, sage, experimental mathematics, simulation, J, recurrence
.. category: 
.. link: 
.. description: 
.. type: text

Here is one `nice problem <https://math.stackexchange.com/questions/1061083/average-time-to-fill-boxes-with-balls/>`_ to describe how arrive at a formula experimentally.

To rephrase the problem:

There are :math:`m` people with one ball each, and :math:`n` boxes. In a round, each of them picks one box randomly (uniformly and independently) and
drops the ball in it. Whichever box is not empty is removed, and the next round starts. How many rounds, on an average, will it take till no
boxes are left?

Obtaining a formula directly by combinatorial arguments without computing any values and getting it right is quite difficult, and prone to errors.

So, let us obtain it experimentally.

What does experimental math involve?

- Brute force through the problem

  - Write a program which describes the problem

  - obtain the first few values

  - take it to either oeis or a sequence guessing routine

  - Then we may be able to find a formula

- If it's a problem on probability, do a simulation to cross verify with the formula

- Having a lot of fun, doing both math and programming at the same time!

Back to our problem, how many rounds can we expect for the game to last? Let us do the simulation by computing answers for small values, in J:

.. code-block:: text
    :number-lines: 0

       'm n'=:5 3
       sim=: 3 : 0
    a=:m
    c=:0
    while. (a>0) do.
    b=:+/~:?n#a
    a=:a-b
    c=:c+1
    end.
    c return.
    )
       (+/%#)(sim"0)1e5#0

Running the above gives a value of about :math:`2.554`

Next, we will try to compute some numbers:
How many ways is it possible for 3 balls to be placed 5 boxes such that everybody chooses the same box?

:math:`abc,0,0,0,0`

:math:`= 5` ways

How many ways is it possible for 3 balls to be placed 5 boxes such that two boxes are selected?

Do some casework:

One box may contain two balls, one box with one ball and one empty box.

:math:`ab,c,0,0,0`

:math:`ac,b,0,0,0`

:math:`bc,a,0,0,0`

:math:`= 3\cdot 5!/3! = 60` ways

How many ways is it possible for 3 balls to be placed 5 boxes such that 3 boxes are selected?

:math:`a,b,c,0,0`

:math:`= 5!/2! = 60` ways

And we see that the total turns out to be :math:`5 + 60 + 60 = 125`, which is 53, the number of ways of arranging the balls in boxes without any restriction.

To calculate the expected value, we have


.. math::

    E[n] = p_1 * E[n-1] + p_2 * E[n-2] + \cdots + p_m * E[n-m]


where :math:`p_m` means the probability of choosing :math:`m` different boxes from :math:`n` boxes.

For :math:`m=3` and :math:`n=5`, :math:`E[5] = 5/125*E[4]+60/125*E[3]+60/125*E[2] + 1`

Then, calculate similarly for :math:`n=4` and :math:`m=3` to get :math:`E[4]` and so on.
The boundary condition is :math:`E[1]=1`, since obviously the game would end in one round if there was a single box.

:math:`E[5]` would be :math:`\dfrac{18391}{7200} = 2.5543` which is close to the simulation. Hence, we can proceed with our experimentation for conjecturing a formula.

Let us calculate the number of ways to partition a number :math:`n` of length :math:`3` (number of people fixed at :math:`m=3`), using sage:

.. code-block:: python
    :number-lines: 0

    def afun(aa,bb,cc):
        nn = aa
        nbac = nn
        mm = bb
        ll = cc
        summ = 0
        alst = Partitions(nn,length=ll).list()
        for a in alst:
            b = a+([0]*(mm-ll))
            tot = 1
            for c in a:
                tot *= binomial(nn,c)
                nn = nn-c            
            summ += Permutations(b).cardinality()*tot
            nn = nbac
        return summ    
    print [afun(i,3,3) for i in range(3,16)]

What this function afun does is that for :math:`n` and :math:`m`, it computes the number of partitions having length :math:`l (\le m)`, and we compute the list of values for :math:`l=m=3` and varying :math:`n`.

Insert that list to oeis, and bingo! The second answer shown looks promising: :math:`A(k,3)` where :math:`A(k,n)= \sum_{m=1}^k (-1)^{m+1}\cdot \binom{n}{m} \cdot m^k`.

It seems to be related to the stirling numbers of the second kind.

After some trial and error, the equation turns out to be:


.. math::

    \displaystyle E_{n,m} &= \left(\sum_{j=1}^{n-1} \left\lbrace {m \atop j} \right\rbrace \dfrac{n!}{(n-j)!} \dfrac{E_{n-j,m}}{n^m}\right)+1\\
    E_{1,m} &= 1

In maxima (which will cache the values to speed up recurrence computation), it can be written as:

.. code-block:: scheme
    :number-lines: 0

    m:3$
    E[1]:1$
    E[n]:=sum(stirling2(m,j)*factorial(n)/factorial(n-j)*E[n-j]/n^m,j,1,n-1)+1$
    float(E[5]);
