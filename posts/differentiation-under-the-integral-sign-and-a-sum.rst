.. title: Differentiation under the integral sign and a sum
.. slug: differentiation-under-the-integral-sign-and-a-sum
.. date: 2014-04-22 15:10:49 UTC+05:30
.. tags: mathjax, integration
.. category: 
.. link: 
.. description: 
.. type: text

We will see how differentiation under the integral sign, which Mr. Feynman loved, can be applied to derive an otherwise difficult integral.
The integral can also be expanded as a Taylor series, thus obtaining an infinite sum.

So, this is the integral:



.. math::

    \displaystyle I(a)=\int_0^1 \, \frac{\ln{(1+a\,(x-x^2))}}{x-x^2}\, dx

Applying the differentiation under the integral sign to this:



.. math::

    \displaystyle 
    \frac{\partial}{\partial a} I(a) &= \int_0^1\, \frac{1}{1+a\,(x-x^2)}\, dx\\ 
    &=\frac{\log\left(\frac{1}{2} \, a + \frac{1}{2} \, \sqrt{a^{2} + 4 \, a} + 1\right)}{\sqrt{a^{2} + 4 \, a}} - \frac{\log\left(\frac{1}{2} \, a - \frac{1}{2} \, \sqrt{a^{2} + 4 \, a} + 1\right)}{\sqrt{a^{2} + 4 \, a}}

Integrating w.r.t :math:`a` gives us (answer by friCAS)



.. math::

    \displaystyle I(a)=\log\left( \frac{a+2 -\sqrt{a^{2} + 4 \, a}}{2}\right)^{2} + C

Putting :math:`a=0` in the above gives :math:`I(0)=0+C` and original integral is also 0. Hence, :math:`C=0` and the final answer to the integral is:



.. math::

    \displaystyle I(a)=\log\left( \frac{a+2 -\sqrt{a^{2} + 4 \, a}}{2}\right)^{2}

Now, we may also expand the integral as a taylor series:


.. math::

    \displaystyle 
    I(a)&=\int_0^1 \, \sum_{n\ge 1}\, \frac{(-1)^{n-1}\, a^{n} (x-x^2)^n}{n\, (x-x^2)} \, dx\\
    &=\int_0^1 \, \sum_{n\ge 1}\, \frac{(-1)^{n-1}\, a^{i} (x-x^2)^{n-1}}{n} \, dx\\
    &=\int_0^1 \, \sum_{n\ge 0}\, \frac{(-1)^{n}\, a^{n+1} (x-x^2)^{n}}{n+1} \, dx\\
    &=\sum_{n\ge 0}\, \frac{(-1)^{n}\, a^{n+1} B(n+1,n+1)}{n+1}\\
    &=\sum_{n\ge 0}\, \frac{(-1)^{n}\, a^{n+1} \Gamma\left(n+1\right)\Gamma\left(n+1\right)}{(n+1)\,\Gamma\left(2\, n+1\right)}\\
    &=\sum_{n\ge 0}\, \frac{(-1)^{n}\, a^{n+1} \, n!^2}{(n+1)\,(2\, n+1)\, (2n)!}\\
    &=\sum_{n\ge 0}\, \frac{(-1)^{n}\, a^{n+1}}{(n+1)\,(2\, n+1)\, \binom{2n}{n}}


Therefore, we have the following sum!



.. math::

    \displaystyle \sum_{n\ge 0}\, \frac{(-1)^{n}\, a^{n+1}}{(n+1)\,(2\, n+1)\, \binom{2n}{n}}=\log\left( \frac{a+2 -\sqrt{a^{2} + 4 \, a}}{2}\right)^{2}

This has got real values for :math:`a>-4`

E.g. when :math:`a=-2`



.. math::

    \displaystyle \sum_{n\ge 0}\, \frac{2^{n+1}}{(n+1)\,(2\, n+1)\, \binom{2n}{n}}=\frac{\pi^2}{4}

and when :math:`a=1/2`



.. math::

    \displaystyle \sum_{n\ge 0}\, \frac{(-1)^{n}}{(n+1)\,(2\, n+1)\, \binom{2n}{n}\, 2^{n+1}}=\log\left(2\right)^{2}
