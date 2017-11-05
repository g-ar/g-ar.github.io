.. title: Optical Illusions with Context Free Art
.. slug: optical-illusions-with-context-free-art
.. date: 2017-11-05 17:38:03 UTC+05:30
.. tags: context free art, computer art, optical illusion
.. category: 
.. link: 
.. description: 
.. type: text

`Context Free Art <https://www.contextfreeart.org>`_ is a program to generate images using context free grammar. We can obtain beautiful images by writing a few tens of lines of code.

To make it more interesting, let's write code for Optical Illusions. There's a `nice list <https://tex.stackexchange.com/questions/129274/showcase-of-optical-illusions-made-with-tex-latex-luatex-context/>`_ already done using LaTeX, below are translations and output of some of those.

CFDG uses ``startshape`` to know which shape to call first. Then the startshape can use other shapes or paths (primitive symbols, user defined).

Read the `documentation <https://github.com/MtnViewJohn/context-free/wiki>`_ for details and examples.

.. code:: text
    :number-lines: 1

    startshape start

    CF::Background = [hue 90 sat -0.5 b -0.5]
    CF::Size = [s 15]

    shape start {
        loop j=6 [] {
        rad = 28 + j*14
        twist = (-1)^j * 12
            loop i=rad [] {
                xs = (j + 2)*cos(i*360/rad)
                ys = (j + 2)*sin(i*360/rad)
                rota = i*360/rad + twist
                if (mod(i, 2) == 1) {
                    sq [b 1 x xs y ys r rota]
                } else {
                    sq [b -1 x xs y ys r rota]
                }
            }
        }
    }

    path sq {
        MOVETO(0, 0)
        LINETO(0, 0.3)
        LINETO(0.3, 0.3)
        LINETO(0.3, 0)
        LINETO(0, 0)
        STROKE(0.03)[]
    }

.. figure:: ../../images/sqr_circles.png

    Illusion 1

.. code:: text
    :number-lines: 1

    startshape start

    CF::Background = [hue 90 sat -0.5 b -0.5]
    n = 8
    xt = (n - 1)/2
    yt = (n - 1)/2
    scale = n + 2
    CF::Size = [s scale x -xt y -yt]
    sqd = 0.8                 // square size
    circd = sqrt(2)*(1 - sqd) // circle size

    shape start {
        loop j=n [] {
            loop i=n [] {
                ys = j + 0.5
                xs = i + 0.5
                SQUARE[b -1 s sqd x i y j]
                if (i < n-1 && j < n-1) {
                    CIRCLE[z 1 b 1 s circd x xs y ys]
                }
            }
        }
    }

.. figure:: ../../images/grid.png

    Illusion 2

Output of illusions `three <https://github.com/g-ar/CFreeArt/blob/master/v3/circles_lines.cfdg>`_, `four <https://github.com/g-ar/CFreeArt/blob/master/v3/floor_tiles.cfdg>`_, and `five <https://github.com/g-ar/CFreeArt/blob/master/v3/grid_lines.cfdg>`_ are shown below

.. figure:: ../../images/circles_lines.png

    Illusion 3

.. figure:: ../../images/floor_tiles.png

    Illusion 4

.. figure:: ../../images/grid_lines.png

    Illusion 5

These examples only used simple loops, more complicated shapes can be drawn using recursion. Check out the `CFDG gallery <https://www.contextfreeart.org/gallery/>`_ for more examples.
