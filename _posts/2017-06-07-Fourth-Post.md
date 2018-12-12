---
layout: post
title: Choosing the benchmark
---

NOTE:*For system specs see [here](https://bhavishyagopesh.github.io/Second-Post/)*

### On choice of "benchmark"

After analysing the results of "perf" module,I decided to work on "Python_startup_time"(which regresses the most and has some activity on [bugs.python.org](https://bugs.python.org/issue29592)).

Here's the relevant stastics once again:
```text
+-------------------------+----------+-------------------------------+
| python_startup          | 19.2 ms  | 42.3 ms: 2.21x slower (+121%) |
+-------------------------+----------+-------------------------------+
| python_startup_no_site  | 9.42 ms  | 26.0 ms: 2.76x slower (+176%) |
+-------------------------+----------+-------------------------------+

```

Next thing is to use `cProfile` to see "who's calling who", some useful points are listed below(if you're interested in full profiles check out [this](https://github.com/bhavishyagopesh/gsoc_2017) github repo.)

Also I generated the `opcodes` but unfortunately they don't provide much insight(they too are in the above mentioned repo)

## NOTE:
- py2 uses **"read method of file object"** which is done away with in py3.It imports io module(the prime reason for it being slow) then TextIOWrapper uses encoding passed by constructor. So following might be the probable solutions:

i)improving time for **'abc module'**(this is a major culprit --our io module uses it)

ii) avoiding import of whole **sysconfig** and only of the variables required.There is a "bpo" thread too on probable solutions like writing a _site builtin module in C OR a C lookup function for accessing sysconfigdata....but there is no consensus yet.

iii) Stopping the import of uncommon modules.

iv) But if the module excluded from startup is very common ,The module will be imported while Python "application" startup anyways,So faster import time is better than avoiding importing for such common modules,Like **"functools, pathlib, os, enum, collections, re."**
