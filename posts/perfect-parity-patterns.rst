.. title: Perfect Parity Patterns
.. slug: perfect-parity-patterns
.. date: 2017-10-30 19:31:51 UTC+05:30
.. tags: sage, fractals, computer art
.. category: 
.. link: 
.. description: 
.. type: text

::

    When image data is involved, the results can be engrossing even if there are bugs in our code.
                                                                                           - D. E. Knuth, TAOCP Vol. 4A

A parity pattern is a binary matrix where each entry is the xor of its adjacent neighbors (horizontal or vertical). It's called perfect when there's no row or column consisting of entirely zeroes.

We can build bigger parity patterns from smaller ones, which reveals nice fractals when they are plotted as a bitmap.

.. code:: python
    :number-lines: 1

    def build_matrix(nbits=5):
        cnt = 0
        for a in range(1, 2^(nbits-1)):        
            con = true                    # continue with the plotting process
            nn = nbits  
            b = Integer(a)
            c = b.bits()
            c = c + [0]*nbits

            am = matrix(ZZ, [c[:nbits]]).stack(zero_matrix(nbits-1, nbits))
            zmv = zero_matrix(ZZ, nn, 1)
            zmh = zero_matrix(ZZ, 1, nn+2)
            am = zmv.augment(am).augment(zmv)
            am = zmh.stack(am).stack(zmh) # consider values outside matrix as 0

            if am[1, :ceil(am.ncols()/2)].list() != am[1, floor(am.ncols()/2):].list()[::-1]: 
                continue                  # Let's have vertical symmetry (palindrome check)

            for i in range(2,nn+1):
                for j in range(1,nn+1):
                    am[i, j] = (am[i-1, j-1] ^^ am[i-1, j] ^^ am[i-1, j+1] ^^ am[i-2, j])

            for j in range(1, nn):
                if am[nn, j] != am[nn-1, j] ^^ am[nn, j-1] ^^ am[nn, j+1]: 
                    con = false           # is the computed matrix actually a parity pattern?
                    break

            if not con:
                continue

            cnt += 1

            nn1 = 2*nn+2
            while nn < 200:               # build 383x383 matrices
                nn1 = 2*nn+2
                bn = zero_matrix(ZZ, nn1)
                for i in range(nn+1):
                    for j in range(nn+1):
                        bn[2*i, 2*j] = am[i, j]
                        bn[2*i+1, 2*j] = am[i, j] ^^ am[i+1, j]
                        bn[2*i, 2*j+1] = am[i, j] ^^ am[i, j+1]
                        bn[2*i+1, 2*j+1] =  0
                bn = bn.delete_rows([0]).delete_columns([0])
                nn = nn1 - 1
                zmv = zero_matrix(ZZ, nn, 1)
                zmh = zero_matrix(ZZ, 1, nn+2)
                am = bn
                am = zmv.augment(am).augment(zmv)
                am = zmh.stack(am).stack(zmh)      
            mp = matrix_plot(bn, frame=False)
            mp.show()
        print cnt

    build_matrix(11)

And we get a bunch of fractals

.. figure:: ../../images/sageR.jpg

    Patterns

Source:

Exercises 190 & 193, 7.1.3, Donald E. Knuth, The Art of Computer Programming, Vol. 4A
