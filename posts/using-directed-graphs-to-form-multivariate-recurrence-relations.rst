.. title: Using Directed Graphs to form multivariate recurrence relations
.. slug: using-directed-graphs-to-form-multivariate-recurrence-relations
.. date: 2014-07-26 16:26:48 UTC+05:30
.. tags: mathjax
.. category: 
.. link: 
.. description: 
.. type: text

There are items comprising of red cubes, blue cubes, and balls;
how many ways can 3 red cubes, 4 blue cubes and 8 balls be arranged in a line so that:

- No three cubes are consecutive

- No three balls are consecutive

- No two red cubes are next to one another

- No two blue cubes are next to one another

How do we go about finding a general formula for :math:`r` red cubes, :math:`b` blue cubes and :math:`c` balls ?

The answer lies in forming a directed graph / Finite Automaton, as we have seen it in one of the previous posts.

Instead of filling in the number of ways, we use the variable names as weights for the valid transitions.

Hence, we can directly solve for the general case by using the following adjacency matrix:


.. math::

    \displaystyle \begin{array}{|l|rrrrrr|}\hline & \mathrm{I} & \mathrm{R} & \mathrm{B} & \mathrm{C} & \mathrm{BR} & \mathrm{CC} \\ \hline \mathrm{I} & 0 & r & b & c & 0 & 0 \\ \mathrm{R} & 0 & 0 & 0 & c & b & 0 \\ \mathrm{B} & 0 & 0 & 0 & c & r & 0 \\ \mathrm{C} & 0 & r & b & 0 & 0 & c \\ \mathrm{BR} & 0 & 0 & 0 & c & 0 & 0 \\ \mathrm{CC} & 0 & r & b & 0 & 0 & 0 \\ \hline \end{array}

which is for the constraints that there's no :math:`RR, BB, RBR, BRB` or :math:`CCC`.

Therefore, for 15 items, we compute


.. math::

    \displaystyle \left(\begin{array}{rrrrrr} 0 & r & b & c & 0 & 0 \\ 0 & 0 & 0 & c & b & 0 \\ 0 & 0 & 0 & c & r & 0 \\ 0 & r & b & 0 & 0 & c \\ 0 & 0 & 0 & c & 0 & 0 \\ 0 & r & b & 0 & 0 & 0 \end{array}\right)^{15}

and extract :math:`[c^8 b^4 r^3]` from the sum of the first row, which is :math:`11394`.

We may also compute the characteristic polynomial of the matrix to get the structure of its multivariate recurrence
relation:


.. math::

    \displaystyle x^6 - (bc + cr)x^4 - (bc^2 + 2bcr + c^2r)x^3 - 2bc^2rx^2 = 0

which is just like the characteristic equation of a recurrence relation, which is:


.. math::

    \displaystyle f_{b,c,r} = f_{b-1,c-1,r}+f_{b,c-1,r-1}+f_{b-1,c-2,r}+f_{b,c-2,r-1}+2 \left(f_{b-1,c-1,r-1}+f_{b-1,c-2,r-1}\right)

and set the required boundary conditions.

Computing the regular expression from the minimized DFA gives us the following multivariate generating function:


.. math::

    \displaystyle G(r,b,c) = \frac{\left(1+c+c^2\right)\left(1+b+r+2br\right)}{1-c\left(1+c\right)\left(b+r+2br\right)}

Reference:

`Putting objects in a line <https://math.stackexchange.com/questions/866503/putting-objects-in-a-line>`_
