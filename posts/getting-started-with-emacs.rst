.. title: Getting Started With Emacs
.. slug: getting-started-with-emacs
.. date: 2016-12-25 11:52:17 UTC+05:30
.. tags: emacs
.. category: 
.. link: 
.. description: 
.. type: text

Emacs is an editor that lets us to edit quickly, avoid boring repetitive typing, define custom commands and bind them to desired keys etc. Plus many of its key bindings work in bash too, so, it's really worth to have some time familiarizing with this ancient editor. Let's take a look about the various key bindings and modes --

A little cheat sheet
--------------------

First, some of the common commands. C means ctrl key, and M means alt key:

.. table::

    +-----------+----------------------------------------------------+
    | **Key**   | **Function**                                       |
    +===========+====================================================+
    | C-e       | Go to end of line                                  |
    +-----------+----------------------------------------------------+
    | C-a       | Go to beginning of line                            |
    +-----------+----------------------------------------------------+
    | C-t       | Swap adjacent characters                           |
    +-----------+----------------------------------------------------+
    | M-t       | Swap adjacent words                                |
    +-----------+----------------------------------------------------+
    | C-x t     | transpose two lines                                |
    +-----------+----------------------------------------------------+
    | C-x k     | kill current buffer                                |
    +-----------+----------------------------------------------------+
    | C-<space> | Mark beginning of region, move cursor to highlight |
    +-----------+----------------------------------------------------+
    | M-%       | replace in region                                  |
    +-----------+----------------------------------------------------+
    | C-s       | forward search                                     |
    +-----------+----------------------------------------------------+
    | C-r       | reverse search                                     |
    +-----------+----------------------------------------------------+
    | M-<       | Go to beginning of buffer                          |
    +-----------+----------------------------------------------------+
    | M->       | Go to end                                          |
    +-----------+----------------------------------------------------+
    | C-x h     | Whole buffer's selected as region                  |
    +-----------+----------------------------------------------------+
    | f3        | Start recording macro                              |
    +-----------+----------------------------------------------------+
    | f4        | stop recording macro (press f4 to play that macro) |
    +-----------+----------------------------------------------------+
    | M-u       | Convert the word after the cursor to uppercase     |
    +-----------+----------------------------------------------------+
    | M-l       | Convert to lower case                              |
    +-----------+----------------------------------------------------+
    | M-c       | Capitalize word                                    |
    +-----------+----------------------------------------------------+
    | M-=       | Count lines, words, and characters in region       |
    +-----------+----------------------------------------------------+

There are thousands of commands, which can be invoked by M-x and entering the command name, in case we don't remember the key binding for the command. C-h can be used to find help.

Org-mode
--------

If anybody has to be instantly impressed by Emacs, this will do it! It's more like a markup language, which can export to several formats like html, LaTeX, rst, plaintext etc., and it's much easier to use than formats like rst. We can even use it to create tables, and apply formulas on them like a spreadsheet. How cool is that, spreadsheet without any of its associated bloat?!

See org-mode's site for its excellent documentation.

Ido-mode
--------

This provides suggestions when entering the command after M-x, which is quite helpful in discovering commands when we vaguely remember the command, or even a new command. And it searches for substrings which needn't be continuous, e.g. pressing ``M-x areg`` will highlight align-regexp

Yasnippet
---------

This is a mode where for a given minor-mode and a keyword in that mode, on pressing tab, it expands to the code snippet as stored. E.g. Open a blank C file, type ``main``, then hit TAB, it gets expanded to

.. code:: C
    :number-lines: 1

    #include <stdio.h>

    int main(int argc, char *argv[])
    {

        return 0;
    }

Auto-complete mode
------------------

Provides suggestions to complete the words, showing frequently typed words on top.

Jedi with python mode
---------------------

With python mode, jedi is useful for autocompletion and code navigation.

Restclient-mode
---------------

This is indispensable when developing web APIs.
Type the method, endpoint, headers, message, and C-c C-c, and a nicely formatted response will be shown in an adjacent window! The requests can be stored in a file, separted by ``#`` as a delimiter.

Expand-region
-------------

Expands the region based on semantic units
E.g. in a word "a-string", when the cursor is at 'a', when ``C-=`` is pressed once, 'a' is selected, on pressing again 'a-string' is selected, pressing again selects '"a-string"'. Very useful when we need to copy or delete a block of code.

Tiny
----

Tiny is not yasnippet, is another one, which can expand a given sentence with required numerical range.
E.g. Construct the ascii table! Press ``M-x tiny-expand`` after entering
``m97\n122|%03d %(string x)``

.. code:: text

    097 a
    098 b
    099 c
    100 d
    101 e
    102 f
    103 g
    104 h
    105 i
    106 j
    107 k
    108 l
    109 m
    110 n
    111 o
    112 p
    113 q
    114 r
    115 s
    116 t
    117 u
    118 v
    119 w
    120 x
    121 y
    122 z

Rainbow-delimiters-mode
-----------------------

This is useful for languages where brackets are used to identify blocks, like lisp, C etc. This mode marks each block level's parentheses with  different colors, so that it becomes easy to figure out any missing parenthesis.

Artist-mode
-----------

Want to make some ascii-art? There's a mode for that as well! 
Enter ``M-x artist-mode``, and then we can easily create rectangle, ellipse, polygons etc. in the text file.
If we want to draw with a mouse in the emacs GUI, press ``shift-<mouse-2>`` (middle click), which shows the menu of options to draw like rectangle, ellipse, pen, spray-can etc. Very handy if we want to draw simple block diagrams.

Calc-mode
---------

This is a stack-based scientific calculator which performs many of the calculations. It's a mini CAS!
To start it, enter ``M-x calc``, and as an example, we can do unit conversions, by typing in the calc window:

.. code:: text

    1 
    u c
    day
    s

will return 86400, which means 86400 seconds are there in a day.

And there are many more modes and commands, which make editing fun!

For a detailed reference:

- `Emacs reference card <https://www.gnu.org/software/emacs/refcards/ps/refcard.ps.gz>`_

- `Org mode <http://orgmode.org/>`_

- `Ido mode <https://masteringemacs.org/article/introduction-to-ido-mode>`_

- `yasnippet <https://www.emacswiki.org/emacs/Yasnippet>`_

- `auto-complete <https://github.com/auto-complete/auto-complete>`_

- `Jedi <https://tkf.github.io/emacs-jedi/latest/>`_

- `Restclient-mode <https://github.com/pashky/restclient.el>`_

- `Expand-region <https://github.com/magnars/expand-region.el>`_

- `Tiny <https://elpa.gnu.org/packages/tiny.html>`_

- `Rainbow-delimiters-mode <https://github.com/Fanael/rainbow-delimiters>`_

- `Calc <https://www.gnu.org/software/emacs/manual/html_mono/calc.html>`_

or get help within emacs, by pressing ``C-h m``
