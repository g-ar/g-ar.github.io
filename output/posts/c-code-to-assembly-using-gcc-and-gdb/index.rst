.. title: C code to assembly using gcc and gdb
.. slug: c-code-to-assembly-using-gcc-and-gdb
.. date: 2014-01-19 23:29:58 UTC+05:30
.. tags: C, assembly, GCC, GDB, fasm
.. category: 
.. link: 
.. description: 
.. type: text

Reading the disassembled code from the C programs which we can comfortably write is a great way to learn assembly language, do some archtecture specific optimizations and also to know what's happening under the hood.

In this post, we will see how to translate a small C program to assembly (using flat assembler).

Consider the following, where the code for gcd is taken from rosetta code:

.. code-block:: C
    :number-lines: 1

    #include <stdio.h>

    int gcd(int u, int v) {
        return (v != 0)?gcd(v, u%v):u;
    }

    int main() {
        int n,m;
        scanf("%d%d",&n,&m);
        printf("%d \n",gcd(n,m));
        return 0;
    }


Compile to 32 bit code as

.. code-block:: bash
    :number-lines: 1

    gcc -o gcd gcd.c -m32

and disassemble:

.. code-block:: bash
    :number-lines: 1

    gdb ./gcd
    (gdb) disas gcd

We will see something like this:

.. code-block:: S
    :number-lines: 1

    0x08048454 <+0>  :    push   ebp
    0x08048455 <+1>  :    mov    ebp,esp
    0x08048457 <+3>  :    sub    esp,0x18
    0x0804845a <+6>  :    cmp    DWORD PTR [ebp+0xc],0x0
    0x0804845e <+10> :    je     0x804847e <gcd+42>
    0x08048460 <+12> :    mov    eax,DWORD PTR [ebp+0x8]
    0x08048463 <+15> :    mov    edx,eax
    0x08048465 <+17> :    sar    edx,0x1f
    0x08048468 <+20> :    idiv   DWORD PTR [ebp+0xc]
    0x0804846b <+23> :    mov    eax,edx
    0x0804846d <+25> :    mov    DWORD PTR [esp+0x4],eax
    0x08048471 <+29> :    mov    eax,DWORD PTR [ebp+0xc]
    0x08048474 <+32> :    mov    DWORD PTR [esp],eax
    0x08048477 <+35> :    call   0x8048454 <gcd>
    0x0804847c <+40> :    jmp    0x8048481 <gcd+45>
    0x0804847e <+42> :    mov    eax,DWORD PTR [ebp+0x8]
    0x08048481 <+45> :    leave
    0x08048482 <+46> :    ret

and

.. code-block:: bash
    :number-lines: 1

    (gdb) disas main


shows like this:

.. code-block:: S
    :number-lines: 1

    0x08048483 <+0>  :    push   ebp
    0x08048484 <+1>  :    mov    ebp,esp
    0x08048486 <+3>  :    and    esp,0xfffffff0
    0x08048489 <+6>  :    sub    esp,0x20
    0x0804848c <+9>  :    mov    eax,0x80485b0
    0x08048491 <+14> :    lea    edx,[esp+0x1c]
    0x08048495 <+18> :    mov    DWORD PTR [esp+0x8],edx
    0x08048499 <+22> :    lea    edx,[esp+0x18]
    0x0804849d <+26> :    mov    DWORD PTR [esp+0x4],edx
    0x080484a1 <+30> :    mov    DWORD PTR [esp],eax
    0x080484a4 <+33> :    call   0x8048380 <__isoc99_scanf@plt>
    0x080484a9 <+38> :    mov    edx,DWORD PTR [esp+0x1c]
    0x080484ad <+42> :    mov    eax,DWORD PTR [esp+0x18]
    0x080484b1 <+46> :    mov    DWORD PTR [esp+0x4],edx
    0x080484b5 <+50> :    mov    DWORD PTR [esp],eax
    0x080484b8 <+53> :    call   0x8048454 <gcd>
    0x080484bd <+58> :    mov    edx,0x80485b5
    0x080484c2 <+63> :    mov    DWORD PTR [esp+0x4],eax
    0x080484c6 <+67> :    mov    DWORD PTR [esp],edx
    0x080484c9 <+70> :    call   0x8048390 <printf@plt>
    0x080484ce <+75> :    mov    eax,0x0
    0x080484d3 <+80> :    leave
    0x080484d4 <+81> :    ret

From the disassembly, we can see that the function arguments are pushed from right to left.
We can also see that the local variables are allocated space in the stack.

We need to replace all the relative references by labels, memory references by names and remove all "PTR" keywords.
Using the example to produce dynamically linked executable from fasm for linux (doing it in 1.70.03), we may write it as:

.. code-block:: S
    :number-lines: 1

    format ELF executable 3
    entry start

    include      'examples/elfexe/dynamic/import32.inc'
    include      'examples/elfexe/dynamic/proc32.inc'

    interpreter  '/lib/ld-linux.so.2'
    needed       'libc.so.6'
    import       printf,scanf,exit

    segment readable executable

    gcd:
        push   ebp
        mov    ebp,esp
        sub    esp,0x18
        cmp    DWORD [ebp+0xc],0x0
        je     l1
        mov    eax,DWORD [ebp+0x8]
        mov    edx,eax
        sar    edx,0x1f
        idiv   DWORD [ebp+0xc]
        mov    eax,edx
        mov    DWORD [esp+0x4],eax
        mov    eax,DWORD [ebp+0xc]
        mov    DWORD [esp],eax
        call   gcd
        jmp    l2
    l1:
        mov    eax,DWORD [ebp+0x8]
    l2:
        leave
        ret

    start:
        push     ebp
        mov      ebp,esp
        and      esp,0xfffffff0
        sub      esp,0x20
        cinvoke  scanf,pars,n,m
        mov      edx,[n]
        mov      eax,[m]
        mov      DWORD  [esp+0x4],edx
        mov      DWORD  [esp],eax
        call     gcd
        cinvoke  printf,parspf,eax
        mov      eax,0x0
        cinvoke  exit

    segment readable writeable
        pars    db '%d%d',0
        parspf  db '%d',0xa,0
        n       dd 0
        m       dd 0

and assemble:

.. code-block:: bash
    :number-lines: 1

    ./fasm gcd.asm

The assembled code will perform the same way, but the executable produced is about 10 times smaller! With the assembly code, we will have more liberty to use architecture specific instructions. And, if we see that there are unnecessary register spills happening, we may modify the code to avoid it. (using "register" keyword and ``-O3`` option in gcc makes good use of registers)

p.s.

- By default, disassembly syntax is not intel. To change it, use

  .. code-block:: bash
      :number-lines: 1

      set disassembly-flavor intel

  You may consider placing it in ``$HOME/.gdbinit`` to use intel syntax everytime.

- ``-m32`` option in gcc is not required if 32 bit linux distro is used.

- ``-g`` option is helpful in debugging the executable. We can check instruction-wise disassembly and also deduce the operator  precedence. You'll never need another silly book on C. When in doubt, go to the root!
