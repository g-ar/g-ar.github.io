.. title: Generating Music With SageMath And Sonic Pi - Examples
.. slug: generating-music-with-sagemath-and-sonic-pi-examples
.. date: 2017-10-21 16:05:16 UTC+05:30
.. tags: python, sage, OSC, Sonic Pi
.. category: 
.. link: 
.. description: 
.. type: text

We will explore some examples to make music using the libraries included in Sage and python data structures

1 Bach's Prelude in C
---------------------

Here's a translation of the Mathematica code at `mathematica.SE <https://mathematica.stackexchange.com/questions/156061/manipulate-a-list-representing-bachs-prelude-in-c>`_, which is quite nice. (`Listen to Bach <https://soundcloud.com/user-591836524/bach-prelude>`_)

.. code:: python
    :number-lines: 1

    bwv846 = [["C", "E", "G", "C5", "E5"],
              ["C", "D", "A", "D5", "F5"],
              ["B3", "D", "G", "D5", "F5"],
              ["C", "E", "G", "C5", "E5"],
              ["C", "E", "A", "E5", "A5"],
              ["C", "D", "Fs", "A", "D5"],
              ["B3", "D", "G", "D5", "G5"],
              ["B3", "C", "E", "G", "C5"],
              ["A3", "C", "E", "G", "C5"],
              ["D3", "A3", "D", "Fs", "C5"],
              ["G3", "B3", "D", "G", "B"],
              ["G3", "Bb3", "E", "G", "Cs5"],
              ["F3", "A3", "D", "A", "D5"],
              ["F3", "Ab3", "D", "F", "B"],
              ["E3", "G3", "C", "G", "C5"],
              ["E3", "F3", "A3", "C", "F"],
              ["D3", "F3", "A3", "C", "F"],
              ["G1", "D3", "G3", "B3", "F"],
              ["C3", "E3", "G3", "C", "E"],
              ["C3", "G3", "Bb3", "C", "E"],
              ["F2", "F3", "A3", "C", "E"],
              ["Fs2", "C3", "A3", "C", "Eb"],
              ["Ab2", "F3", "B3", "C", "D"],
              ["G2", "F3", "G3", "B3", "D"],
              ["G2", "E3", "G3", "C", "E"],
              ["G2", "D3", "G3", "C", "F"],
              ["G2", "D3", "G3", "B3", "F"],
              ["G2", "Eb3", "A3", "C", "Fs"],
              ["G2", "E3", "G3", "C", "G"],
              ["G2", "D3", "G3", "C", "F"],
              ["G2", "D3", "G3", "B3", "F"],
              ["C1", "C2", "G3", "Bb3", "E"]]

    finale = ["C1", "C2", "F3", "A3", "C", "F", "C", "A3", "C", "A3", 
       "F3", "A3", "F3", "D3", "C1", "B1", "G", "B", "D5", "F5", "D5", 
       "B", "D5", "G", "B", "D", "F", "E", "D", ["C1", "C2", "E", "G", "C5"]]

    for i in bwv846:
        note = (i + i[-3:])*2
        for j in note:
            send_message("/trigger/play", j)
            sleep(0.25)

    for j in finale:   
        send_message("/trigger/play", j)
        sleep(0.25)

2 Sound of Cellular Automaton
-----------------------------

Using the idea of `mathematica code <https://stackoverflow.com/a/7592876>`_ using Cellular Automata, we modify the `Cellular Automata's interactive example <https://wiki.sagemath.org/interact/misc#Cellular_Automata>`_ (`Listen to CA <https://soundcloud.com/user-591836524/cellular-automata>`_: 61 iterations, Rule 90)

.. code:: python
    :number-lines: 1

    from numpy import zeros
    from random import randint

    def cellular(rule, N, initial='Single-cell'):
        '''Yields a matrix showing the evolution of a Wolfram's cellular automaton

        rule:     determines how a cell's value is updated, depending on its neighbors
        N:        number of iterations
        initial:  starting condition; can be either single-cell or a random binary row
        '''
        M=zeros( (N,2*N+2), dtype=int)
        if initial=='Single-cell':
            M[0,N]=1
        else:
            M[0]=[randint(0,1) for a in range(0,2*N+2)]

        for j in range(1,N):
            for k in range(0,2*N):
                l = 4*M[j-1,k-1] + 2*M[j-1,k] + M[j-1,k+1]
                M[j,k]=rule[ l ]
        return M[:,:-1]

    def num2rule(number):
        if not 0 <= number <= 255:
            raise Exception('Invalid rule number')
        binary_digits = number.digits(base=2)
        return binary_digits + [0]*(8-len(binary_digits))

    @interact
    def _( initial=selector(['Single-cell', 'Random'], label='Starting condition'), N=input_box(label='Number of iterations',default=31),
           rule_number=input_box(label='Rule number',default=90),
           size = slider(1, 11, label= 'Size', step_size=1, default=6 ), auto_update=False):
        rule = num2rule(rule_number)
        M = cellular(rule, N, initial)
        part = M.transpose()*range(1,N+1)*3
        plot_M = matrix_plot(M, cmap='binary')
        plot_M.show(figsize=[size,size])
        for j in part:
            k = [int(3*(N+1)-i) for i in j if i != 0]
            if len(k) != 0:
                send_message("/trigger/play", k)
                sleep(0.1)

3 HMM Emitting Notes
--------------------

We may even use the notes of pleasant compositions to train Markov Models, which can later keep producing notes probabilistically. E.g. a 3 symbol model looks like: (`Listen to HMM <https://soundcloud.com/user-591836524/hidden-markov-model-emitting-notes>`_)

.. code:: python
    :number-lines: 1

    m = hmm.DiscreteHiddenMarkovModel([[1/3,1/2,1/6],[1/7,3/7,3/7],[1/4,1/4,1/2]], [[1,0,0],[0,1,0],[0,0,1]], [0,1,0], ["F", "Fs", "G"])
    for j in m.sample(100):
        send_message("/trigger/play", j)
        sleep(0.1)
