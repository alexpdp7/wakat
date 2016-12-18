Documenting code
================

One of the goals when writing code is making it as easy to understand as possible. Code documentation is a means to achieve that.

This is a controversial topic. I advocate that ideally, one should achieve to reduce code documentation to the minimum. Code should be understandable without comments, software should be buildable/testable requiring minimal work (and thus, minimal documentation) and so on and so forth. Of course, this is an aspirational goal which is not always achievable.

The following Python 3 program::

    print('Hello world')

is completely self-documenting. Even if you do not know Python, it's trivial to figure out that it prints `Hello world`. No amount of commenting will improve this. Our ideal should be for all of our programs to be like hello world.

However, any experience dealing with coding shows that this is rarely true.

So which documentation should we provide? We can classify it as:

* Implementation documentation. It explains how the code works; which algorithms does it use, what it actually does, etc.
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

Implementation documentation explains what the code actually does. I say that the above example is incorrect because it is *redundant*. Redundancy is bad, because it does not add any value and can "drift". If for some reason, a needs to be increased by two instead of one, surely the programmer will change that, because otherwise the program won't work. However, he can forget to update the documentation, because that has no immediate consequence. Implementation documentation which contradicts the code is painful, as it brings doubt to the reader.

Furthermore, documentation makes the code wordier; you can keep less code on screen, you need more effort to read everything, etc.

So when you should provide implementation documentation?

Complex code
~~~~~~~~~~~~

Sometimes, even if we make our best effort to make clear and easy-to-understand code, some of it can still be hard to grasp. Take for instance the pivoting operation in Quicksort. It takes a list of values such as `[3, 5, 8, 2, 1]`, chooses an element (let's say the first, `3`) and rearranges the list so that elements lower than the pivot come first, then the pivot, then elements greater than the pivot.

A naïve, not in place implementation is pretty easy to read:

.. doctest::

    >>> def pivot(l):
    ...     pivot = l[0]
    ...     rest = l[1:]
    ...     return [e for e in rest if e < pivot] + [pivot] + [e for e in rest if e >= pivot]

    >>> pivot([3, 5, 8, 2, 1])
    [2, 1, 3, 5, 8]
