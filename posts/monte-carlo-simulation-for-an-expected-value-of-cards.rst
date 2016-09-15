.. title: Monte-Carlo simulation for an expected value of cards
.. slug: monte-carlo-simulation-for-an-expected-value-of-cards
.. date: 2014-02-09 20:17:59 UTC+05:30
.. tags: mathjax, simulation, J, expected value
.. category: 
.. link: 
.. description: 
.. type: text

*Suppose we have a standard deck of 52 cards, and we pick 13 cards randomly and arrange them in a row, what is the expected number of adjacent pairs that are of same suit?*

E.g. in ♠♥♥♣♦♣♣♦♦♣♠♠♣ , there are 4 adjacent pairs that are of same suit. On an average, what would be the expected number of such pairs?

Newcomers to probability theory would find such a question a bit tricky. In such a situation, using a computer for simulation/enumeration would ease the job. For this problem, enumeration can yield the exact answer, but getting all the combinations is awkward.

In such cases, simulation is there for our rescue! Even an approximate answer would be sufficient to conjecture a formula.

For the simulation, we will be using a language called `J <https://jsoftware.com>`_. The programs written in J can be very short compared to other well known languages. So, we can focus on the problem at hand instead of the program.

Let's see how it can be used for our simulation (there can be other ways, here’s my shot):

.. code-block:: text
    :number-lines: 0

    a=:13#(i.4)
    sim=: 3 : '+/2=/\(13?52){a'
    (+/%#)(sim "0) 1000000#0

That's it, less than 70 characters! It output :math:`2.82489` for me. When we are proceeding with an analytical method, if we get an answer around :math:`2.82`, then we can be pretty sure that it's right.

Some explanation about the program:

In J, every operation is performed right to left, if no parentheses are provided.
So, if we write ``2*5+3``, answer would be ``16`` and not ``13``. No operator precedence here.

In J terminology, the operators are called verbs. They can be monadic or dyadic. Check the help files for more info.

1. ``i.4`` is array of integers ``0 1 2 3``, representing 4 suits. ``13#`` repeats each element 13 times.

2. ``sim`` is the function for our simulation. Read it right to left. ``{`` is the verb for indexing.

   - ``(13?52)`` gets 13 random integers in ``0..51`` without replacement, to simulate 13 card draws.

   - ``2=/\`` compares two adjacent values from the selected list, and returns 1 or 0 for true or false, respectively.

   - ``+/`` gives us the sum of the array, which is the total number of pairs with same adjacent suit.

3. " is a verb for rank. "0 gets atomic values in the rhs, i.e. a million zeros, the rhs is not used in our simulation, it's just for performing the experiment a million times. The outcome of each experiment is then averaged, by using the tacit definition ``(+/%#)``. This last line almost always remains the same for any similar kind of simulation.

Lastly, experiment with different number of picks and observe how answer is changing according to that.

Now, for the analytical result, the linearity of expectation is used.

The probability that the card in positions i and i+1 are of the same suit is given by


.. math::

    \displaystyle \mathbb{P}[c_i=c_{i+1} | c_i \text{ is spade}] = \frac{13}{52}\cdot \frac{12}{51}

and similarly for three other suits. The total probability is then


.. math::

    \displaystyle \mathbb{P}[c_i=c_{i+1}] = \frac{13}{52}\cdot \frac{12}{51} \times 4 = \frac{4}{17}

This is also the expected number of pairs of the same suit when two cards are picked. We write the expected value as


.. math::

    \displaystyle \mathbb{E}[c_i=c_{i+1}] = \frac{4}{17}

Hence, by linearity of expectation, when ‘n’ cards are picked, we can expect


.. math::

    \displaystyle \sum_{i=1}^{n}\mathbb{E}[c_i=c_{i+1}] = \frac{4\, (n-1)}{17}

adjacent pairs to be of the same suit.

For :math:`n=13`, it would be :math:`48/17`, or :math:`2.82352941176471` cards, which agrees with the simulation.
