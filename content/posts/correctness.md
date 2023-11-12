+++
title = "Thoughts on incorrectness and separation logic"
author = ["Monish Singhal"]
date = 2023-11-13T00:00:00+05:30
tags = ["formal-verfication"]
draft = false
+++

<!--more-->

1.  If you have ever browsed through the function definitions of, say, `memcpy`, you will find the `restrict` keyword. But what does it do? Borrowing an example from the ISO C standard:
    ```c
       void f(int n, int * restrict p, int * restrict q) {
           while (n-- > 0)
               *p++ = *q++;
       }
    ```
    Here, the `restrict` keyword hints the compiler that `p` and `q` hold mutually exclusive data: i.e. the data that is accessed through `p` can be accessed through `q`. If I understood separation logic correctly, `restrict` provides some semblance of separation. However, without knowledge of the length of the buffer, it becomes impossible to actually use the full power of separation logic in C.
2.  I was reading [the paper on incorrectness logic](https://dl.acm.org/doi/10.1145/3371078) the other day. Incorrectness logic flips Hoare logic on it's head, by considering a subset of all possible `post(C)` conditions instead of the superset that is considered in Hoare logic. In other words, while Hoare logic has no false negatives (it finds all possible outcomes and more), incorrectness logic has no false positives (every outcome it finds will be reachable from some input state). This turns out to be rather useful in bug-finding, because often we care about reachable states that can lead to error conditions from specific starting states. Often, due to the domain of inputs (for example, distributed systems), it might even be impossible for Hoare logic to throw up useful output.

    It may be possible to use incorrectness logic to reverse engineer binaries. It fits the requirement pretty well: find some pathological input case that yields a bug. However, some work would be required to operate on ASM code instead of C/Java code that is used in Facebook Infer.
