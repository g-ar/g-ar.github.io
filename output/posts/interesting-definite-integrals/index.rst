.. title: Interesting definite integrals
.. slug: interesting-definite-integrals
.. date: 2014-05-04 15:16:30 UTC+05:30
.. tags: mathjax, integration
.. category: 
.. link: 
.. description: 
.. type: text

We will see a list of few integrals which yields to differentiation under the integral sign:

1. .. math::

       \displaystyle \int_0^\infty \, \frac{\arctan{(a\,(x^n))}}{x^{n}}\, dx = \frac{a^{1-\frac{1}{n}} {\rm B}\left(\frac{1}{2 \, n}, 1-\frac{1}{2 \, n}\right)}{2 \, {\left({n} - 1\right)}}\\ =\frac{\pi \, a^{1-\frac{1}{n}}}{2 \, {\left({n} - 1\right)} \, \sin\left(\frac{\pi}{2n}\right)}

   or a more general result, for :math:`k \ge n`



   .. math::

       \displaystyle \int_0^\infty \, \frac{\arctan{(a\,(x^k))}}{x^{n}}\, dx =\frac{\pi \, a^{\frac{n-1}{k}}}{2 \, {\left({n} - 1\right)} \, \sin\left(\frac{k-n+1}{2\, k}\, \pi\right)}

2. .. math::

       \displaystyle \int_0^{\pi/2} \, \log{\left(1+a\, \sin(x)^{2}\right)}\, dx = \pi\, \log{\left(\frac{\sqrt{a+1}+1}{2}\right)}

3. .. math::

       \displaystyle \int_0^{\infty} \, e^{-a\sqrt{x}}\, \frac{\sin{\sqrt{x}}}{x}\, dx = \pi - 2\, \arctan{a}

4. .. math::

       \displaystyle \displaystyle \int_0^{\infty} \, \frac{\log{\left(1+\frac{a}{x^{3}}\right)}}{1+x^{3}}\, dx = \frac{1}{3\, \sqrt{3}} \, {\left(2 \, \sqrt{3} \pi \arctan\left(\frac{1}{\sqrt{3}} \, {\left(2 \, a^{\frac{1}{3}} + 1\right)}\right) + 3 \, \pi \log\left(a^{\frac{2}{3}} + a^{\frac{1}{3}} + 1\right)\right)}-\frac{\pi^{2}}{9}

5. .. math::

       \displaystyle \displaystyle \int_0^{\infty} \, \frac{x\, \log{\left(1+\frac{a}{x^{3}}\right)}}{1+x^{3}}\, dx = \frac{\pi^{2}}{9}-\frac{1}{3\, \sqrt{3}} \, {\left(2 \, \sqrt{3} \pi \arctan\left(\frac{1}{\sqrt{3}} \, {\left(2 \, a^{\frac{1}{3}} + 1\right)}\right) - 3 \, \pi \log\left(a^{\frac{2}{3}} + a^{\frac{1}{3}} + 1\right)\right)}

6. .. math::

       \displaystyle \int_0^{\infty} \, \log\left(\frac{a^{2} x^{2} + 1}{a^{2} + 1}\right)\, \frac{1}{1-x^{2} }\, dx = -\arctan(a)^2

7. .. math::

       \displaystyle \int_0^{1} \frac{\log\left(1+a x \right)}{x\, \sqrt{1-x^{2}} }\, dx = \frac{1}{2}\, \log{\left(\sqrt{a^2-1}+a\right)}^{2}+\frac{\pi^2}{8}

8. .. math::

       \displaystyle \int_{0}^{\infty}\, \frac{\log\left(a x + b\right)}{{\left(x + 1\right)}^{2}}\, dx = \frac{a \log\left(a\right) - b \log\left(b\right)}{a - b}

9. .. math::

       \displaystyle \int_{0}^{\infty}\, \frac{e^{-a x^{2}} \sin\left(b\, x\right)^{2}}{x^{2}}\, dx = \frac{1}{2} \, \sqrt{\pi} {\left(b\, \sqrt{\pi}\, \text{erf}\left(\frac{b}{\sqrt{a}}\right) + \sqrt{a} e^{-b^{2}/a}\right)} - \frac{1}{2} \, \sqrt{\pi\, a}

10. This integral is different. The rule is applied twice, and integrated back twice, finding the constant of integration each time.


    .. math::

        \displaystyle \int_{0}^{\infty}\, \frac{e^{\left(-a x\right)} \sin\left(b x\right)^{5}}{x^{2}}\, dx = -\frac{3}{16} \, \pi a - \frac{5}{32} \, b {\left(\log\left(25\right) - 3 \, \log\left(9\right)\right)} + \frac{5}{8} \, a \arctan\left(\frac{a}{b}\right) - \frac{5}{16} \, a \arctan\left(\frac{a}{3 \, b}\right) + \frac{1}{16} \, a \arctan\left(\frac{a}{5 \, b}\right) - \frac{5}{32} \, b \log\left(\frac{a^{2} + 25 \, b^{2}}{25 \, b^{2}}\right) + \frac{15}{32} \, b \log\left(\frac{a^{2} + 9 \, b^{2}}{9 \, b^{2}}\right) - \frac{5}{16} \, b \log\left(\frac{a^{2} + b^{2}}{b^{2}}\right)

11. More complicated than the previous, the rule is to be applied 5 times:


    .. math::

        \displaystyle \int_{0}^{\infty}\, \frac{e^{\left(-a x\right)} \sin\left(b x\right)^{5}}{x^{5}}\, dx = \frac{1}{128} \, \pi a^{4} + \frac{5}{64} \, \pi a^{2} b^{2} + \frac{115}{384} \, \pi b^{4} - \frac{5}{192} \, a^{4} \arctan\left(\frac{a}{b}\right) + \frac{5}{32} \, a^{2} b^{2} \arctan\left(\frac{a}{b}\right) - \frac{5}{192} \, b^{4} \arctan\left(\frac{a}{b}\right) + \frac{5}{384} \, a^{4} \arctan\left(\frac{a}{3 \, b}\right) - \frac{45}{64} \, a^{2} b^{2} \arctan\left(\frac{a}{3 \, b}\right) + \frac{135}{128} \, b^{4} \arctan\left(\frac{a}{3 \, b}\right) - \frac{1}{384} \, a^{4} \arctan\left(\frac{a}{5 \, b}\right) + \frac{25}{64} \, a^{2} b^{2} \arctan\left(\frac{a}{5 \, b}\right) - \frac{625}{384} \, b^{4} \arctan\left(\frac{a}{5 \, b}\right) + \frac{5}{192} \, a^{3} b \log\left(25\right) - \frac{5}{64} \, a^{3} b \log\left(9\right) - \frac{125}{192} \, a b^{3} \log\left(a^{2} + 25 \, b^{2}\right) + \frac{45}{64} \, a b^{3} \log\left(a^{2} + 9 \, b^{2}\right) - \frac{5}{96} \, a b^{3} \log\left(a^{2} + b^{2}\right) + \frac{5}{96} \, a^{3} b \log\left(\frac{a^{2}}{b^{2}} + 1\right) - \frac{5}{64} \, a^{3} b \log\left(\frac{a^{2}}{9 \, b^{2}} + 1\right) + \frac{5}{192} \, a^{3} b \log\left(\frac{a^{2}}{25 \, b^{2}} + 1\right)

    So, if :math:`a=0`, it reduces to a simpler form:


    .. math::

        \displaystyle \int_{0}^{\infty}\, \frac{\sin\left(b x\right)^{5}}{x^{5}}\, dx =\frac{115}{384} \, \pi b^{4}

    In Sage, it can be applied as:

    .. code-block:: python
        :number-lines: 1

        # Takes a few seconds
        var('a b')
        forget()
        assume(a>0)
        intg = e^(-a*x)*sin(b*x)^5/x^5
        tmp = integrate(integrate(diff(intg,a,5),x,0,oo),a)
        h1(a) = tmp-limit(tmp,a=oo)
        tmp = integrate(h1(a),a)
        h2(a) = tmp-limit(tmp,a=oo)
        tmp = integrate(h2(a),a)
        h3(a) = tmp-limit(tmp,a=oo)
        tmp = integrate(h3(a),a)
        h4(a) = tmp-limit(tmp,a=oo)
        tmp = integrate(h4(a),a)
        h5(a) = tmp-limit(tmp,a=oo)

12. .. math::

        \displaystyle \int_{0}^{\pi/2}\, e^{-b\, \sec{(x)}^{2}} \, \sin{(a\, \tan{x})}^{2} \, dx \\ =\frac{\pi}{4}\left(1 - e^{-2 \, a}\right) -\frac{1}{8} \, \pi {\left({\rm erfc}\left(\frac{a}{\sqrt{b}} + \sqrt{b}\right) e^{\left(4 \, a\right)} + {\rm erfc}\left(-\frac{a}{\sqrt{b}} + \sqrt{b}\right) - 2\right)} e^{\left(-2 \, a\right)} - \frac{1}{4} \, \pi \text{erf}\left(\sqrt{b}\right)

    or in particular, when :math:`b \to 0`,


    .. math::

        \displaystyle \int_{0}^{\pi/2}\, \sin{(a\, \tan{x})}^{2} \, dx =\frac{\pi}{4}\left(1 - e^{-2 \, a}\right)

13. .. math::

        \displaystyle \int_{0}^{\infty}\, \frac{\log{(1+a\, x^3)}}{(1-x+x^2)}\, dx = \frac{2}{\sqrt{3}} \, \pi \, \log\left(1+ a^{\frac{1}{3}} + a^{\frac{2}{3}} \right)

14. .. math::

        \displaystyle \int_{0}^{1}\, \frac{\arctan{a\, x}}{x\, \sqrt{1-x^2}}\, dx = \frac{\pi}{2} \, \sinh^{-1}{a}

15. .. math::

        \displaystyle \int_{0}^{1}\, \frac{\log{\left(1+a\, x\, (1-x)\right)}}{x}\, dx = \sum_{i=1}^{\infty}\frac{(-1)^{i-1} a^{i}}{i^{2}\, \binom{2i}{i}} = \frac{1}{2}\, \log\left( \frac{a+2 -\sqrt{a^{2} + 4 \, a}}{2}\right)^{2}\\ \implies \sum_{i=1}^{\infty}\frac{1}{i^{2}\, \binom{2i}{i}} =\frac{\pi^{2}}{18} \\ \phantom{\implies}\sum_{i=1}^{\infty}\frac{2^{i}}{i^{2}\, \binom{2i}{i}} =\frac{\pi^{2}}{8} \text{ etc.}

16. .. math::

        \displaystyle \int_{0}^{\pi}\, \log{\left(1-2\, a\, \cos{x} + a^{2}\right)}\, dx = 2\, \pi\, \log{|a|}

17. .. math::

        \displaystyle \int_0^{\pi/4} \frac{\left(\log{\tan{\left(\frac{\pi}{4}+ x\right)}}\right)^n}{\tan{(2x)}}\, dx = \frac{n!}{2^n} \left(1-\frac{1}{2^{n + 1}}\right) \zeta(n + 1)

18. .. math::

        \displaystyle \int_0^1 \log\frac{\big(x+a\, \sqrt{1-x^2}\big)^2}{\big(x-a\, \sqrt{1-x^2}\big)^2} \frac{x\, dx}{1-x^2} = 2\,\pi\arctan{a}

19. The functions in the answer are Beta and the Polygamma:


    .. math::

        \displaystyle \int_0^{\pi/2}\, \frac{\sin(x)^a\, \log{\sin{x}}}{\sqrt{1+\sin(x)^2}}\, dx = \frac{1}{16} \, {\left(\psi_0\left(\frac{a+1}{4} \right)-\psi_0\left(\frac{a+3}{4} \right) \right)} {\rm B}\left(\frac{a+1}{4},\frac{1}{2}\right)
