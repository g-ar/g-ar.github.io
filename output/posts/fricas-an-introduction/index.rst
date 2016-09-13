.. title: FriCAS -- an introduction
.. slug: fricas-an-introduction
.. date: 2014-06-18 15:35:29 UTC+05:30
.. tags: mathjax
.. category: 
.. link: 
.. description: 
.. type: text


There are quite a few different free computer algebra systems around :math:`-` Sage, maxima, sympy, FriCAS/Axiom etc. :math:`-` each having its strengths and lack of something. Having choices and access to source code are good, since we are not at mercy of vendors like those big M developers for bug fixes and feature additions (along with outrageous licence costs).

I have been using FriCAS for a while mainly for its number sequence guessing routines, an indispensable part when working on enumeration problems. But recently, when I tried other operations which I mostly do in Sage or maxima, like integration and solving equations, I was surprised to see it could give simpler and more complete answers than Sage/maxima.

One more good thing is that it comes with a fricas mode for emacs, which has many more features compared to running from a terminal. E.g. it gives features like auto-completion, matched-parenthesis highlighting, shortcut keys for navigating through the input, yanking text etc., and of course, we can define our own shortcuts since it's emacs! Let us see how to set FriCAS up and run from emacs in linux. The latest version at the time is 1.2.3, and I have only tried amd64 binary version.

Make a directory ``$HOME/bin/`` if not already there, and add to the ``$PATH`` environment variable. Extract the components to ``$HOME/bin/``. To run it, we need to modify a few paths in its files. Go to ``$HOME/bin/usr/local/bin``, in fricas file, update the variable ``exec_prefix`` to

.. code-block:: sh
    :number-lines: 0

    exec_prefix="${FRICAS_PREFIX:-/home/bin/usr/local}"

In file efricas, update FRICASCMD to

.. code-block:: sh
    :number-lines: 0

    FRICASCMD='/home/bin/fricas'   

and also update the line which calls emacs.

Update the function fricas :math:`-` run in ``$HOME/bin/usr/local/lib/fricas/emacs/fricas.el`` to

.. code-block:: scheme
    :number-lines: 0

    (defun fricas-run ()
      "Run FriCAS in the current BUFFER."
      (message "Starting FriCAS...")
      (start-process-shell-command "fricas" (current-buffer)
                                   fricas-run-command
                                   "-noclef" "2>/dev/null"))

Otherwise, FriCAS won't start within emacs.

Next, create two bash scripts within ``$HOME/bin/`` with filenames "fricas" :math:`-` which is to execute ``$HOME/bin/usr/local/bin/fricas``, and "efricas" to execute ``$HOME/bin/usr/local/bin/efricas``. Make those two newly created files as executable. There, we are all set now. Simply open the terminal, and enter "efricas" to run fricas within emacs. If everything goes well, we will have fricas running within emacs.

Now, let us have a brief overview of its commands (some are examples taken from the axiom book),
and its advantages to other free CAS, and probably even the paid ones.

1) **INTEGRATION**

   .. code-block:: text
       :number-lines: 0

       integrate(tan(atan(x)/3),x)

   This integral is an example mentioned in their document, which is instantly solved by fricas, but Sage/maxima fails after trying for a long time.

2) 
   .. code-block:: text
       :number-lines: 0

       integrate((x+a)^(1/2)/x,x)

   This gives two results, for negative and non-negative a.

   Hence, besides having a good capability, another advantage over Sage and maxima is that we need not declare the symbols which will be used in operations. It also computes the results for all possible cases, and doesn't nag us to make assumptions like in the case of Sage/maxima. (sometimes it keeps asking for the same assumption even if we have already done so!)

3) **SOLVING EQUATIONS**

   .. code-block:: text
       :number-lines: 0

       solve(x^3+x+1,1/1000)
       solve(x^3+x+1,1/1000.0)
       radicalSolve(x^3+x+1)
       complexSolve(x^3+x+1,1/1000.0)

   etc. All of the above call the same algorithm to compute the roots, but the result is returned depending on the data type.

   It can also solve non-linear simultaneous equations.

4) 
   .. code-block:: text
       :number-lines: 0

       solve([x+y^2-4,x^2+y-2],1.E-10)
       solve([x+y^2-4,x^2+y-2],1/10^10)
       radicalSolve([x+y^2-4,x^2+y-2])

   We can see that it can give all the exact results also effortlessly. In Sage/maxima, there is currently no way of making itto output all results in form of radicals.

5) **RECURRENCE RELATIONS**

   The recursions are transformed into iterated code and compiled! And since it can also symbolically compute, this proves very useful to examine sequences.

   .. code-block:: text
       :number-lines: 0

       fib(0)==0
       fib(1)==1
       fib(n)==fib(n-1)+fib(n-2)

   This automatically compiles and computes the fibonacci numbers as an iterated code.

6) 
   .. code-block:: text
       :number-lines: 0

       a(0)==1
       a(1)==1
       a(2)==1
       b(0)==0
       b(1)==0
       b(2)==0
       a(n)==a(n-1)+b(n-1)
       b(n)==a(n-3)+b(n-3)

   This simultaneous recurrence is actually narayana's cows sequence, and this kind of recurrence is compiled as well!
   Using this, we will make use of the guessing routines.

7) **GUESS**

   The guessing routine in FriCAS can give us the likely generating function, recurrence relation, functional equation etc.

   Using the above simultaneous recurrence, the generating function can be obtained as:

   .. code-block:: text
       :number-lines: 0

       guessAlg [a(i)+b(i) for i in 0..20]

8) Try for the recurrence relation.

   .. code-block:: text
       :number-lines: 0

       guessPRec [a(i)+b(i) for i in 0..20]

   This command gives a single recurrence relation! Hence, we may solve a problem our way and use the guessing routines for simplification.
   Series expansion

   Working with series is also in a way different and easy.

9) 
   .. code-block:: text
       :number-lines: 0

       series(x/(1-x-x^2),x=0)

   or like this

   .. code-block:: text
       :number-lines: 0

       x:=series 'x
       x/(1-x-x^2)

10) If we require only the list of coefficients of the series

    .. code-block:: text
        :number-lines: 0

        cf:=coefficients x/(1-x-x^2)

    If we want the :math:`[x^{100}]`

    .. code-block:: text
        :number-lines: 0

        coefficient(x/(1-x-x^2),100)

    or

    ``cf.200``

11) **SOME MISCELLANEOUS INFO**

    Since the output is always pretty-printed and does not provide a way to turn it off (though there are options to output different formats like TeX, fortran, html etc.), we can obtain an unparsed output the following way:

    .. code-block:: text
        :number-lines: 0

        k:=(-b)^(1/3)/(1+b)
        unparse(k :: InputForm)

12) Shell commands can be executed within it:

    .. code-block:: text
        :number-lines: 0

        )system pwd
        )system date

    etc.

13) **CHANGE OUTPUT FORMAT**

    .. code-block:: text
        :number-lines: 0

        )set output tex on
        )set output tex abc.tex

14) **SHOW TIME FOR EXECUTION**

    .. code-block:: text
        :number-lines: 0

        )set messages time on

15) Sage provides an interface to FriCAS, so we may even run it within Sage.

    .. code-block:: text
        :number-lines: 0

        fricas('series(1/sqrt(1-x),x=0)')

and there are many more. It even has its own language :math:`-` SPAD.

For more details, see

`1. Axiom book <http://fricas.sourceforge.net/doc/book.pdf>`_

`2. FriCAS sandbox <http://axiom-wiki.newsynthesis.org/SandBoxFriCAS>`_

and of course, the source code is available to know "everything" about it!
