.. title: Solving Mastermind-like Problems Using Z3 Theorem Prover
.. slug: solving-mastermind-like-problems-using-z3-theorem-prover
.. date: 2020-04-27 23:22:20 UTC+05:30
.. tags: mathjax, Z3, SMT, SAT, puzzle, mastermind, numbermind
.. category: 
.. link: 
.. description: 
.. type: text

| *4 7 2 9 1 - One number is correct but not in right position*
| *9 4 6 8 7 - One number is correct but not in right position*
| *3 1 8 7 2 - Two numbers are correct but only one is in right position*
| *1 5 7 3 9 - Two numbers are correct and both in right position*

| *Assuming all the digits are distinct, what is the 5-digit number?*
| (Asked on `puzzling stackexchange <https://puzzling.stackexchange.com/questions/97032/5-digit-puzzle-code-looking-for-solution>`_)



This can be quite easily solved using Z3 theorem prover, once we figure out to set up the constraints.

We have 5 variables, :math:`\{x_i\}, i=0, 1, 2, 3, 4`
which are constrained by 


.. math::

    \displaystyle
    0 \le x_i &\le 9 \\
    x_i \neq x_j \land i &\neq j\\
    [x_0 \neq 4] + [x_1 \neq 7] + [x_2 \neq 2] + [x_3 \neq 9] + [x_4 \neq 1] &= 5\\
    [x_0 \neq 9] + [x_1 \neq 4] + [x_2 \neq 6] + [x_3 \neq 8] + [x_4 \neq 7] &= 5\\
    [x_0 \neq 3] + [x_1 \neq 1] + [x_2 \neq 8] + [x_3 \neq 7] + [x_4 \neq 2] &= 4\\
    [x_0 \neq 1] + [x_1 \neq 5] + [x_2 \neq 7] + [x_3 \neq 3] + [x_4 \neq 9] &= 3\\

where :math:`[\cdot]` is the Iverson Bracket notation (1 if the condition holds, 0 otherwise).

When checking the models for which the conditions are satisfied, we will also check that the number of digits satisfying the conditions holds good, using set intersection.

We solve it using python bindings to Z3, which can be installed in python virtual environment by

.. code:: bash
    :number-lines: 1

    pip install z3-solver

Then, we can solve the above by running the following python code:

.. code:: python
    :number-lines: 1

    from z3 import *

    def main():
        sol = Solver()
        cols = 5

        a = [[4,7,2,9,1],
             [9,4,6,8,7],
             [3,1,8,7,2],
             [1,5,7,3,9]]

        # variables
        x = [Int("x[%d]" % j) for j in range(cols)]

        # constraints
        '''
        All are distinct
        '''
        sol.add(Distinct(x))

        '''
        All numbers 0 <= x_i <= 9
        '''
        for i in range(cols): 
            sol.add(x[i] >= 0, x[i] <= 9)

        sol.add(Sum([If(x[c] != a[0][c],1,0) for c in range(cols)]) == 5) # no number in correct position
        sol.add(Sum([If(x[c] != a[1][c],1,0) for c in range(cols)]) == 5) # no number in correct position
        sol.add(Sum([If(x[c] != a[2][c],1,0) for c in range(cols)]) == 4) # one number in correct position
        sol.add(Sum([If(x[c] != a[3][c],1,0) for c in range(cols)]) == 3) # two numbers in correct position

        cnt = 0

        while sol.check() == sat:
            mod = sol.model()
            xval = [mod.eval(x[j]).as_long() for j in range(cols)]
            if len(set(xval) & set(a[0])) == 1 and\
               len(set(xval) & set(a[1])) == 1 and\
               len(set(xval) & set(a[2])) == 2 and\
               len(set(xval) & set(a[3])) == 2:    
                cnt += 1
                print(xval)
            sol.add(Or([x[i] != mod.eval(x[i]).as_long() for i in range(cols)])) # add constraint to check for different solution

        print("#solutions: ", cnt)

    if __name__ == "__main__":
        main()
    '''
    [6, 5, 0, 3, 2]
    #solutions:  1
    '''

If we remove the ``distinct`` constraint, we get 19 different solutions.

We find that Z3 and other SMT solvers are powerful tools for verification computer programs / hardware designs, perform compiler optimizations, finding bugs etc. And solving Logic puzzles!

References:

1. `Z3 Theorem Prover wiki <https://en.wikipedia.org/wiki/Z3_Theorem_Prover>`_

2. `HakanK's page on Z3 <http://www.hakank.org/z3/>`_

3. `A guide to Z3 <https://ericpony.github.io/z3py-tutorial/guide-examples.htm>`_
