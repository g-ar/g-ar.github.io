.. title: Josephus problem -- C versus J
.. slug: josephus-problem-c-versus-j
.. date: 2014-03-26 12:00:59 UTC+05:30
.. tags: mathjax, C, J
.. category: 
.. link: 
.. description: 
.. type: text

In this post, we will see about implementation of a famous problem -- the Josephus Problem.

As you can see, there's already an implementation in mathematica. But, not many of us are rich enough to buy MM. So, we need to make use of awesome and free programming languages like C and J! C for fast execution, J for quick coding.

First, to implement in C, we need to write our own data structures. We will make use of a circular linked list, since that's how the josephus problem is stated.

.. code-block:: C
    :number-lines: 1

    #include <stdio.h>
    #include <stdlib.h>

    struct node *cur,*tmp,*head;
    struct node
    {
       int num;
       struct node *next;
    };

    void kill(int n, int k)
    {
        struct node *prev;
        while(k>1){
            prev=head;
            head=head->next;
            k--;
        }
        tmp=head->next;
        free(head);
        prev->next=tmp;
        head=tmp;
    }
    int main()
    {
        int n,k,i;

        scanf("%d",&n);
        scanf("%d",&k);
        head=(struct node*)malloc(sizeof(struct node));
        head->num=1;
        tmp=head;
        for(i=2;i<=n;i++)
        {
            cur=(struct node*)malloc(sizeof(struct node));
            cur->num=i;
            tmp->next=cur;
            tmp=cur;
        }
        cur->next=head;
        while(n>1){
            kill(n, k);
            n--;
        }
        printf("%d\n",head->num);
        return 0;
    }

For the original Josephus problem, :math:`n=41` and :math:`k=3`, and the program answers :math:`31` as expected.
Next, we move on to J:

J being an array programming language, there are verbs to operate on arrays. So, the josephus problem is a natural candidate for J. Here it goes:

.. code-block:: text
    :number-lines: 1

    a=:1+i.41
    jose=: 4 : 'a=:}.(x-1)|.y'  NB. rotate and delete the first item
    3 jose^:(_1+#a) a

There, very much smaller than the C version. J code above uses nesting of function, i.e. the function jose is applied 40 times on array 'a', and a single element remains after that.

A more explicit version can be written as:

.. code-block:: text
    :number-lines: 1

        jos =: 4 : 0
    while. (#y)>1 do.
    y=.}.(x-1)|.y
    end.
    return. y
    )
        3 jos 1+i.41

When n is small, the execution time does not make much difference either in J or C. But we can notice the difference clearly when n is large. E.g. for :math:`n=40000` and :math:`k=11`, the time taken in J is about 1.68s and in C, it's only about 0.018s in my machine. J can be sped up a bit by operating on multiple items at a time, and makes the program a little more complicated.

You may wish to compare this with your favorite language's implementation in rosettacode, which has several clever solutions.
