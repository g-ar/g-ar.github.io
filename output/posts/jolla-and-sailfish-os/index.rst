.. title: Jolla and sailfish OS
.. slug: jolla-and-sailfish-os
.. date: 2014-11-09 16:34:48 UTC+05:30
.. tags: mathjax
.. category: 
.. link: 
.. description: 
.. type: text


Last week, I bought a Jolla phone after looking at some of the amazing features it offers. What are those amazing features? We'll see..

Most of the reviews scattered on the net miss the fact that it is a fully functional linux machine! We can install gcc, python etc. from the terminal (after the developer mode is activated). Let me list some of the things I did after getting my hands on the Jolla phone:

- I didn't have an access point for wifi, so used hostapd to create one.

- Create a Jolla account to download the required softwares. (Phone's very much usable even without a sim card)

- After creating the account, we'll be able to enable the developer mode.

- Now, the terminal will be displayed.

- Open the terminal, become a root by typing in ``devel-su`` and the password from the settings->system->developer-mode

- We can also login via ssh, which will make our typing a lot easier. I connected via WLAN's IP address (``ssh nemo@ip-addr``, and type the password)

- gcc, g++, gdb, python and strace can be installed by issuing commands like ``pkcon install gcc``

- Since we are also interested to do some math, what are our options?

  - I downloaded sagemath for arm :math:`-` ``sage-5.13-armv7l-Linux``, and tried to run it. Too bad, it crashed saying ``ImportError: libcblas.so.3: cannot open shared object file: No such file or directory`` (Update: The sage 6.0 binary available for RPi works for jolla too, woohoo! Only issue I found till now is that plotting did not work.)

  - For FriCAS, Axiom, maxima etc., binaries are not available for ARM. To compile them, they require a lisp compiler, which isn't available in the repos yet (ecl is available in openrepos.net, did not try it though).

  - But anyway, if we look into `openrepos <https://openrepos.net>`_, we find sympy, numpy, IPython, and matplotlib, which are more than sufficient for a mobile phone!

- Why use IPython? Because, it provides us with a notebook interface which can be opened remotely, and also tab completion feature

- We can also install several useful softwares like emacs, system monitor, cpufrequtils etc. from openrepos.net. Hence, we can have the tools we need with us all the time! (Emacs calc mode itself can serve as a mini CAS!)

- The history of events like sms and calls are stored in a database. We can use SQL commands to extract the event of interest. E.g.

  .. code-block:: sh
      :number-lines: 0

      sqlite3 .local/share/commhistory/commhistory.db

- Then, to display all the sms messages, run commands like

  .. code-block:: sql
      :number-lines: 0

      select * from events where type=2;

- If we want to know what each column is about, run

  .. code-block:: sql
      :number-lines: 0

      .headers ON

- And to top it all, though an ARM binary is not available, we can easily compile J to run it from the console! I think it's perfectly suitable language that can be used on a phone. (J download for RPi works too)

To conclude, it's totally developer friendly. If anyone likes to program but hates all those manifest files seen in other mobile OSes, it feels like bliss! No need of bloated SDKs, no need of emulators (though it's there if a UI is required), just a simple ssh to connect to the phone and start coding!

It's friendly towards "end users" too, don't believe the "reviews" that say it has a "steep" learning curve because of its gesture based controls. The tutorial it displays when the phone is started is more than enough to understand the Sailfish OS. I won't talk about its UI, since pretty much all the other reviews are about only that and apps, pfff.

Nokia made a big mistake and died by dumping MeeGo and embracing m$.

I wish Jolla team a big success, but I also wish that they remove the mandatory signing up of a Jolla account for enabling developer mode.

`1. Creating AP using hostapd <http://nims11.wordpress.com/2012/04/27/hostapd-the-linux-way-to-create-virtual-wifi-access-point/>`_

`2. Run IPython remotely <https://stackoverflow.com/questions/24490278/run-ipython-notebook-from-a-remote-server>`_

`3. Sage 6.0 for RPi download <https://github.com/ArchimedesPi/SageMathematics-raspi/>`_
