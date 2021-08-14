.. title: Solving a Puzzle Using Picat
.. slug: solving-a-puzzle-using-picat
.. date: 2021-08-14 18:30:30 UTC+05:30
.. tags: Picat, SAT, puzzle, CP
.. category: 
.. link: 
.. description: 
.. type: text

*Arrange the numbers 1-32, inclusive, in a circle such that the sum of any two adjacent numbers in the circular chain is a perfect square*

| Picat is a programming language for solving constraint problems, logic problems etc.
| It's quite fast too compared to python z3 solver.
| So, here's one way to solve the circle problem in picat

.. code:: text
    :number-lines: 1

    main => go.

    go =>

       % decision variables
       N = 32,
       Xs = new_list(N),

       % given constraints
       Xs :: 1..N,
       Sq = [I**2 : I in 2..7],

       % fix one number, to avoid rotation
       Xs[1] #= 1, 
       foreach (I in 1..N-1),
           sum([(Xs[I+1]+Xs[I]) #= Sq[J] : J in 1..Sq.length]) #= 1, 
       end,
       sum([(Xs[N]+Xs[1]) #= Sq[J] : J in 1..Sq.length]) #= 1, 
       all_different(Xs),

       % solve and print
       Res = solve_all(Xs),

       foreach (R in Res),
          println(R),
       end,
       nl.

References:

1. `Picat Page <http://picat-lang.org/download.html>`_

2. `HakanK's page on Picat <http://www.hakank.org/picat/>`_
