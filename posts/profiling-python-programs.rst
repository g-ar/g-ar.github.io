.. title: Profiling Python Programs
.. slug: profiling-python-programs
.. date: 2017-01-17 17:56:38 UTC+05:30
.. tags: python, strace
.. category: 
.. link: 
.. description: 
.. type: text

Profiling Python Programs
-------------------------

Last week, when I was looking up for some info on list comprehensions, one of the pages listed a code something like this

.. code:: python
    :number-lines: 1

    # Access every 3rd element in a list
    # filename: access.py
    def main():
        a = [1]*10
        i = 0
        while i < len(a):
            print a[i]
            i = i + 3

    if __name__ == "__main__":
        main()

and I thought for a moment whether the length will be calculated for each iteration, or will it be called only once? I also thought of an answer that since python code is interpreted, it may not do any peephole optimization, and hence ``len`` will be called five times. Anyway, wanted to know the command that would quantify every call in the program and found about cProfile module. We simply run the following:

.. code:: bash
    

    python -mcProfile -sncalls access.py

which will do the profiling and order by the number of calls

.. code:: text
    

    1
    1
    1
    1
             8 function calls in 0.000 seconds

       Ordered by: call count

       ncalls  tottime  percall  cumtime  percall filename:lineno(function)
            5    0.000    0.000    0.000    0.000 {len}
            1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
            1    0.000    0.000    0.000    0.000 access.py:2(<module>)
            1    0.000    0.000    0.000    0.000 access.py:2(main)

So, will the optimization flag do anything for that?

.. code:: python
    

    # filename: call_access.py
    import access

    if __name__ == "__main__":
        access.main()

and run

.. code:: bash
    

    python -OO -mcProfile -sncalls call_access.py

but the output doesn't change much!

.. code:: text
    

    1
    1
    1
    1
             9 function calls in 0.000 seconds

       Ordered by: call count

       ncalls  tottime  percall  cumtime  percall filename:lineno(function)
            5    0.000    0.000    0.000    0.000 {len}
            1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
            1    0.000    0.000    0.000    0.000 access.py:2(<module>)
            1    0.000    0.000    0.000    0.000 access.py:2(main)
            1    0.000    0.000    0.000    0.000 call_access.py:1(<module>)

So, even with the optimization flag, number of calls to ``len`` remains the same.

We can verify that the bytecode is executed when we run it by using ``strace``. For instance, when I ran command the second time, the relevent part of the ``strace`` output is below

.. code:: text
    

    open("access.py", O_RDONLY)             = 3
    fstat(3, {st_mode=S_IFREG|0644, st_size=172, ...}) = 0
    open("access.pyo", O_RDONLY)            = 4
    fstat(4, {st_mode=S_IFREG|0644, st_size=389, ...}) = 0
    read(4, "\3\363\r\nC\252}Xc\0\0\0\0\0\0\0\0\2\0\0\0@\0\0\0s#\0\0\0d\0"..., 4096) = 389
    fstat(4, {st_mode=S_IFREG|0644, st_size=389, ...}) = 0
    read(4, "", 4096)                       = 0
    close(4)                                = 0
    close(3) 

It did not read access.py again, but it read access.pyo instead. As long as the source is not modified, python reads the bytecode to execute instead of the source (the bytecode stores the modification time of its source code)

Anyway, we need not do any of these analyses to know whether the bytecode has any code optimization in it. The python docs straight away answers with the following point

- A program doesn't run any faster when it is read from a .pyc or .pyo file than when it is read from a .py file; the only thing that's faster about .pyc or .pyo files is the speed with which they are loaded. (`docs <https://docs.python.org/2/tutorial/modules.html>`_)
