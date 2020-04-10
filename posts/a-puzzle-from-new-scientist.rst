.. title: A puzzle from New Scientist
.. slug: a-puzzle-from-new-scientist
.. date: 2020-04-10 22:25:50 UTC+05:30
.. tags: sage, puzzle
.. category: 
.. link: 
.. description: 
.. type: text

*It is Tom's 7th birthday and he has a cake with seven candles on it, arranged in a circle -- but they are trick candles. If you blow on a lit candle, it will go out, but if you blow on an unlit candle, it will relight itself.*

*Since Tom is only 7, his aim isn't brilliant. Any time he blows on a particular candle, the two either side also get blown on as well. How can Tom blow out all the candles? What is the fewest number of puffs he can do it in?*

This is a nice candidate for the shortest path problem, which can be easily solved in Sagemath as follows:

.. code:: python
    :number-lines: 1

    mat = [[0]*128 for i in range(128)]

    '''
    Build an adjacency matrix, where three consecutive bits of the adjacent nodes are toggled
    '''
    for i in range(128):
        mat[i][i^^0b111] = 1
        mat[i][i^^0b1110] = 1
        mat[i][i^^0b11100] = 1
        mat[i][i^^0b111000] = 1
        mat[i][i^^0b1110000] = 1
        mat[i][i^^0b1100001] = 1
        mat[i][i^^0b1000011] = 1

    '''
    Find the shortest path, from all-lit candles to none
    Display the seven candles in binary
    '''
    mx = Matrix(mat)
    g = DiGraph(mx)
    [bin(i)[2:].zfill(7) for i in g.shortest_path(u=127, v=0)]
    '''
    ['1111111', '1110001', '0010000', '1100000', '0100011', '0011011', '0000111', '0000000']
    '''

