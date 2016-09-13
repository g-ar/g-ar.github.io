.. title: Installing and Running TensorFlow in Debian Wheezy
.. slug: installing-and-running-tensorflow-in-debian-wheezy
.. date: 2015-11-11 17:16:08 UTC+05:30
.. tags: mathjax
.. category: 
.. link: 
.. description: 
.. type: text

A couple of days ago google released what they term it as the second generation deep learning neural network software system, called TensorFlow. Google has been taking much interest in deep learning since two or three years (perhaps having surplus computation power?), and they are using it for all sorts of softwares they develop :math:`-` maps, translation, spying on you for targeted ads etc. So, I thought of trying out their newly released tool.

First, I tried to install it using their "easy way" using ``virtualenv`` and ``pip``. Getting into the virtual environment is easy, and pip install command also worked, but when importing tensorflow, it spit an ugly ImportError :math:`-` ``/lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.17' not found``. Now, for the other option.

Next, I thought installing from sources would definitely go well. Downloaded all the source codes and dependencies required by the project, and bazel was one of them. Downloaded bazel as well, but when the installation script was run, that method also went kaput with some error complaining about the kernel version or something. Running from a virtual machine or installing some latest distro was my only option, I thought. But no! There's another method using docker image, which I overlooked even though that was mentioned at the top of the installation page.

Here are the steps:

- Docker runs only on 64-bit OS with kernel version 3.10 or higher. So, the first step must be to install the latest kernel available from wheezy backports. Follow the backport instructions

- Then reboot the machine selecting the latest kernel version

- Follow the docker installation steps

- Now, we are all set to install TensorFlow. Simply run

  - ``sudo docker run -it b.gcr.io/tensorflow/tensorflow``

- That will pull all the required dependencies and set the docker image (which was about 300MiB to download)

- Wouldn't it be convenient if we could add some data persistance, to save data so that the host could use it? There's an option for that as well

  - ``sudo docker run -it -v /host/dir:/docker/dir b.gcr.io/tensorflow/tensorflow``

- Now the ``/host/dir`` is mounted to the ``/docker/dir`` when the image is run

- And that's it, no need of a virtualbox, which wastes more time and space than to run a docker image

- Check an example program from the tutorial, like plotting mandelbrot set
