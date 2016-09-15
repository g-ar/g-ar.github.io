.. title: Compiling softwares that require later versions of GCC
.. slug: compiling-softwares-that-require-later-versions-of-gcc
.. date: 2015-12-20 19:40:01 UTC+05:30
.. tags: mathjax, GCC, context free art, computer art, debian, linux
.. category: 
.. link: 
.. description: 
.. type: text

Suppose we require to install the latest version of a software, but the repository doesn't have it, we usually either get the latest source code and compile, or download the compiled binary if available, and run it. But if we are running some old distro, latest binary refuse to run complaining that it requires GLIBC 2.16 to run etc. When the binary file refuses to run, we can then try to compile the source, which usually works fine. But sometimes it happens that the source requires a language standard, which is unsupported by the GCC version installed. In such cases, we can download the latest GCC, compile it, and then compile the required software using the latest GCC. We'll see an example to compile "Context Free" -- a program which generates art from a context free grammar, and requires C++11 standard to compile. The steps mentioned below were done on debian wheezy, whose software repository contains version 2 of cfdg whereas the latest version is 3.

In order to compile the latest version of contextfree in debian wheezy:

- Download the latest GCC

- Extract and configure with a prefix path, so that we can avoid installing it to a directory which requires root privilege

- ``./configure --prefix=$HOME/bin/custom_gcc``

- The compile and install to that path, the whole process takes much time anywhere between 10 mins to a couple of hours or more, depending on the speed of the machine and the number of cores used. 
  To compile on a single core, use

  - ``make``

  - ``make install``

After GCC is installed, we need to indicate in the makefile of contextfree to use that version of GCC. So, add the following lines to the makefile, install ``libpng12-dev`` and compile.

.. code-block:: bash
    :number-lines: 0

    CC = $(HOME)/bin/gcc_custom/bin/gcc
    LD = $(HOME)/bin/gcc_custom/bin/gcc
    CPP = $(HOME)/bin/gcc_custom/bin/g++
    CXX = $(HOME)/bin/gcc_custom/bin/g++

The output cfdg can then be used as a standalone file. But we need to export some paths to run it, otherwise it'll complain about the older libraries. To get around that, we can create a small bash script to export the compiler and library paths and then run cfdg. E.g. create a script in ``$HOME/bin`` named cfdg with following contents

.. code-block:: bash
    :number-lines: 0

    #!/bin/bash
    export CC=$HOME/bin/gcc_custom/bin/gcc
    export CXX=$HOME/bin/gcc_custom/bin/g++
    export LD_LIBRARY_PATH=$HOME/bin/gcc_custom/lib64
    $HOME/bin/context-free-3.0.9/cfdg "$@"

and ``chmod +x cfdg``, and have fun with the software!
