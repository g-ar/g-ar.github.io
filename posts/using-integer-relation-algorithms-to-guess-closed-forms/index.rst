.. title: Using integer relation algorithms to guess closed forms
.. slug: using-integer-relation-algorithms-to-guess-closed-forms
.. date: 2016-07-02 19:51:29 UTC+05:30
.. tags: mathjax, pslq, integration, experiment math
.. category: 
.. link: 
.. description: 
.. type: text

In this post, we'll see about guessing closed forms of the answers obtained by numerical methods.
In particular, we'll use the excellent math toolkit by David Bailey et. al. aimed at experimental mathematics -- `arprec <http://crd.lbl.gov/~dhbailey/mpdist/arprec-2.2.18.tar.gz>`_. 
Compile and run mathtool.

The following problems are taken from `Brilliant <https://brilliant.org>`_


1. .. math::

       \displaystyle \int_0^{\frac{\pi}{3}} x \left(\ln{\left(2 \sin{\frac{x}{2}}\right)}\right)^2 \, dx = \frac{c\, \pi^a}{b}

   Find $a, b,$ and :math:`c`.

   Start mathtool, and enter the commands in sequence:
   (Only the relevent output is shown after command executions)

   .. code-block:: text
       :number-lines: 0

       integrate[x*log[2*sin[x/2]]^2,{x,0,pi/3}]
       -- snip --
       > 0.25554854129290762855238976168333131037737175253636607542005616591624
       epsilon=-50
       pslq[0.25554854129290762855238976168333131037737175253636607542005616591624, table[pi^i,{i,2,4}]]
       -- snip --
       > Relation:  0 =
       > +  6480.* pslq001
       > +     0.* pslq002
       > +     0.* pslq003
       > +   -17.* pslq004
       > Result[ 37] through Result[ 40] =
       >      6480.00000000000000000000000000000000000000000000000000000000000000000000
       >       0.00000000000000000000000000000000000000000000000000000000000000000000e0
       >       0.00000000000000000000000000000000000000000000000000000000000000000000e0
       >       -17.00000000000000000000000000000000000000000000000000000000000000000000

   So, the above output means, :math:`0= 6480\times 0.255548541292907628552389761683331310377371752536366075420056165916 - 17\times \pi^4`, and hence :math:`c=17, a=4, b=6480`

   When doing such computations, it's good to have more digits when computing integral or sums, and reduce the epsilon value when using pslq, so that it checks for fewer decimal places when trying for an integer relation. Otherwise, it is likely to miss the relation when the numerical accuracy is kept high. digits=100 and epsilon=-50 worked well for me in most cases.

2. .. math::

       \displaystyle \int_0^{2\log{\phi}} \log{\left(2\, \sinh{\frac{x}{2}}\right)} = -\frac{\pi^a}{b}

   .. code-block:: text
       :number-lines: 0

       epsilon=-100
       integrate[log[2*(exp[x/2]-exp[-x/2])/2],{x,1e-100,2*log[(1+sqrt[5])/2]}]
       > -0.98696044010893586188344909998761511353136994072407906264133493762200
       epsilon=-50
       pslq[-0.98696044010893586188344909998761511353136994072407906264133493762200, table[pi^i,{i,1,4}]]
       > Relation:  0 =
       > +  10.* pslq001
       > +   0.* pslq002
       > +   1.* pslq003
       > +   0.* pslq004
       > +   0.* pslq005
       > Result[ 29] through Result[ 33] =
       >        10.00000000000000000000000000000000000000000000000000000000000000000000
       >       0.00000000000000000000000000000000000000000000000000000000000000000000e0
       >         1.00000000000000000000000000000000000000000000000000000000000000000000
       >       0.00000000000000000000000000000000000000000000000000000000000000000000e0
       >       0.00000000000000000000000000000000000000000000000000000000000000000000e0

   In this example, mathtool chokes when lower limit is 0 saying argument is too large, so keep it close to 0. Then using pslq, we see that a=2 and b=10.

3. .. math::

       \displaystyle \int_{-\infty}^{\infty} \dfrac{\log{\left(1 + e^{2x}\right)}}{1 + e^{3\, x}} = \frac{a \pi^b}{c}

   .. code-block:: text
       :number-lines: 0

       epsilon=-100
       Integrate[Log[1 + Exp[2*x]]/(1 + Exp[3* x]), {x, -Infinity, Infinity}]

   If we try to execute it directly, it complains "argument too large". So, we transform it by substituting :math:`e^x=y`

   .. code-block:: text
       :number-lines: 0

       Integrate[Log[1 + y^2]/(1 + y^3)/y, {y, 0, Infinity}]
       > 0.59400396858408176872614992128884242944017635321356610251561824949472
       epsilon=-50
       pslq[0.59400396858408176872614992128884242944017635321356610251561824949472,table[pi^i,{i,1,5}]]
       > Relation:  0 =
       > +  216.* pslq001
       > +    0.* pslq002
       > +  -13.* pslq003
       > +    0.* pslq004
       > +    0.* pslq005
       > +    0.* pslq006
       > Result[  7] through Result[ 12] =
       >       216.00000000000000000000000000000000000000000000000000000000000000000000
       >       0.00000000000000000000000000000000000000000000000000000000000000000000e0
       >       -13.00000000000000000000000000000000000000000000000000000000000000000000
       >       0.00000000000000000000000000000000000000000000000000000000000000000000e0
       >       0.00000000000000000000000000000000000000000000000000000000000000000000e0
       >       0.00000000000000000000000000000000000000000000000000000000000000000000e0

   Hence, :math:`a=13, b=2, c=216`

4. .. math::

       \displaystyle \int_0^1 \log{x}\log{\left(1-x\right)} dx = \frac{a}{b}-\frac{\pi^c}{d}

   .. code-block:: text
       :number-lines: 0

       epsilon=-100
       integrate[log[x]*log[1-x],{x,0,1}]
       > 0.35506593315177356352758483335397481078105009879320156226444177062999
       epsilon=-50
       pslq[0.35506593315177356352758483335397481078105009879320156226444177062999, 1,table[pi^i,{i,1,5}]]
       > Relation:  0 =
       > +  -6.* pslq001
       > +  12.* pslq002
       > +   0.* pslq003
       > +  -1.* pslq004
       > +   0.* pslq005
       > +   0.* pslq006
       > +   0.* pslq007
       > Result[ 27] through Result[ 33] =
       >        -6.00000000000000000000000000000000000000000000000000000000000000000000
       >        12.00000000000000000000000000000000000000000000000000000000000000000000
       >       0.00000000000000000000000000000000000000000000000000000000000000000000e0
       >        -1.00000000000000000000000000000000000000000000000000000000000000000000
       >       0.00000000000000000000000000000000000000000000000000000000000000000000e0
       >       0.00000000000000000000000000000000000000000000000000000000000000000000e0
       >       0.00000000000000000000000000000000000000000000000000000000000000000000e0

   Therefore, :math:`a=2, b=1, c=2, d=6`
