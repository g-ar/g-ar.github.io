.. title: Configuring Broadband Connection using pppoeconf
.. slug: configuring-broadband-connection-using-pppoeconf
.. date: 2012-11-14 22:51:48 UTC+05:30
.. tags: mathjax
.. category: 
.. link: 
.. description: 
.. type: text

It all started when the lightning struck and disrupted the internet service. Previously, the settings were stored in the router itself, but after the incident it was no longer connecting to the ppp server.

I called the ISP, and the router's configuration was changed by the support guy and since he's familiar only with windoze, I had to reboot it to windows xp (which I no longer use, only linux based OS now). Now, in order to connect to the internet, the username/password needs to be given in the dial-up prompt for the broadband connection (pppoe). But, for some weird reason, DNS was working, but no ping reply from any sites!

So I decided to try in linux mint, and thought of using pppoeconf.

It's straight-forward, for my ISP's connection the default settings worked. The only information which I needed to change was the username/password

To my surprise, ping replies were okay, but the DNS look up was not working now. After tinkering around with the files in ``/etc/ppp`` and reading some man pages related to ppp, I discovered an info referring to ``/etc/resolv.conf`` file. But, there was no such file in ``/etc`` by default.

So, I copied the ``/etc/ppp/resolv.conf`` to ``/etc``

Surprise again, it was still not working. Then I tried ``strace nslookup google.com``

Somewhere near the end of the output, I found ``open("/etc/resolv.conf", O_RDONLY) = -1 EACCES (Permission denied)``. So, ``sudo chmod 0644 /etc/resolv.conf``, and rebooted the system.

Everything worked fine after that.


Update: Actually, we need not worry about configuring from the terminal at all. With network manager already there, we just need to go to "network connections"->"DSL"  in the menu, and add our connection id/passwd.
