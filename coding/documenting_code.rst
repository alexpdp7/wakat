Documenting code
================

One of the goals when writing code is making it as easy to understand as possible. Code documentation is a means to achieve that.

This is a controversial topic. I advocate that ideally, one should achieve to reduce code documentation to the minimum. Code should be understandable without comments, software should be buildable/testable requiring minimal work (and thus, minimal documentation) and so on and so forth. Of course, this is an aspirational goal which is not always achievable.
 
The following Python 3 program::

    print('Hello world')

is completely self-documenting. Even if you do not know Python, it's trivial to figure out that it prints `Hello world`. No amount of commenting will improve this. Our ideal should be for all of our programs to be like hello world.

However, any experience dealing with coding shows that this is rarely true.

So which documentation should we provide? We can classify it as:
 
* Implementation documentation. It explains how the code works; which algorithms does it use, why it is implemented that way, etc.
* Interface documentation. It explains how to use some unit; which parameters has a function and what does it return, what a module/class purpose is and how to use it, etc.
* Architecture documentation. It explains how the code is structured, which parts it has and how they assemble to become a complete program.
* Hacking documentation. It explains how to build and test the code.
* Deploying documentation. It explains how to deploy/distribute the code.
* Usage documentation. It explains end-users how to use the code.

There are many other different kinds of documentation that we will not cover here.

Implementation documentation
----------------------------

Generally, this is provided inline with the code. Let's start with an incorrect example::

    // increase a by one
    a = a + 1

What does this comment achieve? Nothing. If anything, it increases our cognitive load and clutters the screen. More insidiously, it introduces the possibility that someone modifies the code but forgets to update the comment, adding confusion to whomever reads the code.

So when you should provide implementation documentation? When there is something worth adding.

Complex code
~~~~~~~~~~~~

Sometimes, even if we make our best effort to make clear and easy-to-understand code, some of it can still be hard to grasp. Take for instance the pivoting operation in Quicksort. It takes a list of values such as `[3, 5, 8, 2, 1]`, chooses an element (let's say the first, `3`) and rearranges the list so that elements lower than the pivot come first, then the pivot, then elements greater than the pivot.

A naïve, not in place implementation is pretty easy to read [#incompletepivot]_:

.. doctest::

    >>> def pivot(l):
    ...     pivot = l[0]
    ...     rest = l[1:]
    ...     return [e for e in rest if e < pivot] + [pivot] + [e for e in rest if e >= pivot]

    >>> pivot([3, 5, 8, 2, 1])
    [2, 1, 3, 5, 8]

This reads easily: we pivot on the value of the first element, and the pivoted list is the elements of the rest of the list lower than the pivot, followed by the pivot, followed by the elements of the rest of the list greater or equal to the pivot. This is ideal, as it does not require comments to be understood.

However, this implementation is inefficient; it creates many lists consuming much more memory than strictly needed. This can be avoided by rearranging elements within the list to pivot without allocating new arrays [#sourceinplacepivot]_:

.. doctest::

    >>> def pivot(l):
    ...     p = 0
    ...     for i in range(1, len(l)):
    ...         if l[i] <= l[0]:
    ...             p = p + 1
    ...             l[i], l[p] = l[p], l[i]
    ...     l[p], l[0] = l[0], l[p]

    >>> l = [3, 5, 8, 2, 1]
    >>> pivot(l)
    >>> l
    [1, 2, 3, 5, 8]

(note that this does not return the same list than the naïve implementation, it can shuffle some elements in each partition)

However, the more efficient implementation is much less clear than the naïve implementation. This is a common situation, as if the clearest implementation is not efficient, then a more efficient implementation will not be as clear!

We should always prefer the clearest piece of code which solves the problem. Only if it's proven to be unacceptably inefficient we should move to more efficient, less clear implementations. But then we will be forced to add comments to make it readable:

.. doctest::

    >>> def pivot(l):
    ...     # We choose the first element in the list as the pivot
    ...
    ...     # We use p to track where the pivot will end up; that is
    ...     # initially its original place
    ...     p = 0
    ...     for i in range(1, len(l)):
    ...         # if the element should go before the pivot...
    ...         if l[i] <= l[0]:
    ...             # we put it before the place where the pivot
    ...             # will be and move the final pivot position to 
    ...             # the right
    ...             p = p + 1
    ...             l[i], l[p] = l[p], l[i]
    ...     # Finally, we put the pivot in its final place
    ...     l[p], l[0] = l[0], l[p]

    >>> l = [3, 5, 8, 2, 1]
    >>> pivot(l)
    >>> l
    [1, 2, 3, 5, 8]

; note that what the code does is explain the purpose of the variables which is not initially obvious. We should prefer using descriptive variable names, but in this case `final_pivot_position` would make the code unwieldy.

Another technique is to split functions in smaller functions with descriptive names, but in this case it isn't much good either.

Motivation
~~~~~~~~~~

Another kind of valuable implementation documentation explains the "why". Following the example above, it would also be worth adding a comment explaining why the in-place implementation was needed when the naïve implementation is so much simpler.

Basically, every time you have an inner monologue like "oh, I will do `xxx` *because* `yyy`", you should capture that in a comment.

.. rubric:: Footnotes
.. [#incompletepivot] This function (and the more efficient implementation following it) is not suitable for implementing quicksort; it only operates on the entire list (and it would need to operate on sections of the list) and does not return the position of the pivot- both concerns have been omitted for brevity and clarity.
.. [#sourceinplacepivot] This has been adapted from http://stackoverflow.com/a/27461889
