.. title: Interactive Regexp and Replacing Regexp with Emacs
.. slug: interactive-regexp-and-replacing-regexp-with-emacs
.. date: 2018-02-25 15:03:57 UTC+05:30
.. tags: emacs, regular expression
.. category: 
.. link: 
.. description: 
.. type: text


If we need to replace text based on patterns, then ``replace-regexp`` is the command.

But we may not always be sure that the regular expression (RE) entered does what we wanted. Not to worry, emacs has it covered: with ``re-builder``! As we keep entering the text, the editor progressively highlights the text in the buffer that matches. Once we are certain that all the required text has matched, we can move to enter the command to replace it.

We'll look at a usecase where we need to uppercase some sql keywords, and cast the output of a function as float



    .. raw:: html

       <script src="https://asciinema.org/a/L6opRuaNH22ZbzdxHmlbDDRoR.js" id="asciicast-L6opRuaNH22ZbzdxHmlbDDRoR" async data-autoplay="true" data-speed="2" width="800">

In ``replace-regexp``, the matched expressions are grouped using ``\(`` ... ``\)``, and the matched groups can be accessed in the result as ``\1``, ``\2``, etc.



    .. raw:: html

       <script src="https://asciinema.org/a/FPnVQNSBV0obVx2jgyDUJjchd.js" id="asciicast-FPnVQNSBV0obVx2jgyDUJjchd" async data-speed="2" width="800">
