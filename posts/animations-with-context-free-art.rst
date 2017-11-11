.. title: Animations with Context Free Art
.. slug: animations-with-context-free-art
.. date: 2017-11-11 20:28:56 UTC+05:30
.. tags: context free art, computer art, moire patterns
.. category: 
.. link: 
.. description: 
.. type: text

Context Free Art can also be used to create animations, and here let's see how to generate some moire patterns.

To generate each frame, cfdg uses the ``time`` parameter, and the ``ftime()`` function gets the value for that frame. For our first example, we generate moire patterns with circles.

.. code:: text
    :number-lines: 1

    startshape start

    CF::Time = [time -2 2]
    CF::Background = [b 1]
    CF::Size = [s 3]

    path circ(number rad) {
        MOVETO(rad,0)
        ARCTO(-rad,0,rad, CF::ArcCW)
        ARCTO(rad,0,rad, CF::ArcCW)
        STROKE(0.01)[]
    }

    shape moire {
        loop 70 [s 0.98]
            circ(1)[]
    }

    shape start {
        moire[x ftime() time -2 2 s 1]
        moire[x -ftime() time -2 2 s 1]
    }

To compile and generate the video of the animations

.. code:: bash
    :number-lines: 1

    cfdg moire_circles.cfdg -s 1080 -a 15x25 -o ta%f.png

generates 375 images (15 seconds, 25 fps) for the animation

.. code:: bash
    :number-lines: 1

    ffmpeg -r 25 -i ./ta%3d.png -c:v libx264 out.mp4

creates the HD video having a frame rate of 25.

And here's the final output



    .. youtube:: nPbH9a-jz2w
       :align: center

Similarly, we can create a couple more animations like moire patterns by `rotating grates <https://github.com/g-ar/CFreeArt/blob/master/v3/moire_lines.cfdg>`_ and `rotating two graphene layers <https://github.com/g-ar/CFreeArt/blob/master/v3/moire_graphene.cfdg>`_



    .. youtube:: GNHMPBs9Ozo
       :align: center

    .. youtube:: uEw5a-Quk6Q
       :align: center
