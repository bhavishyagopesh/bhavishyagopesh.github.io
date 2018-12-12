---
layout: post
title: First suggestion on improving startup time?
---

NOTE:*For system specs see [here](https://bhavishyagopesh.github.io/Second-Post/)*

## What to do exactly for improving performance?

Till recently(week ago), I was only focusing on parts which came up with big time-shares in cProfile but couldn't really figure out...exactly what needs to done.So I asked around a bit on *python-dev* irc channel,suggesting what I had observe and I got help from **Inada Naoki**,he too was working on python-performance.

## Observation?

It was clear that imports take time...so either do not import OR make the import faster(..by writing the module, to be imported, in C ).So for a start [this]( https://gist.github.com/methane/74f6ad95fd6c6ad4dbb32e35012773bf
) patch helps finding the import time of modules(on startup with both site option and without site option.) To see import go to [this](https://github.com/bhavishyagopesh/gsoc_2017/blob/master/python_startup_time/import_times) repo.

It still is a bit cryptic but we can see modules that take the most time(for atomically...),So NOTE->
```Text
* encodings + encodings.utf_8 + encodings.latin_1 took 2.5ms
* io + abc + _wakrefset took 1.2ms
* _collections_abc took 2.1ms
* sysconfig + _sysconfigdata took 0.9ms

```

## Real-work?

If you analyse the `cProfile`(I posted in an earlier post) you'll realise following regarding `_weakrefset` module.

```text
ncalls tottime percall cumtime percall filename:lineno(function)
56    0.000    0.000    0.000    0.000 _weakrefset.py:36(__init__)
14    0.000    0.000    0.000    0.000 _weakrefset.py:58(__iter__)
10    0.000    0.000    0.000    0.000 _weakrefset.py:26(__exit__)
19    0.000    0.000    0.000    0.000 _weakrefset.py:70(__contains__)
10    0.000    0.000    0.000    0.000 _weakrefset.py:20(__enter__)
15    0.000    0.000    0.000    0.000 _weakrefset.py:81(add)
10    0.000    0.000    0.000    0.000 _weakrefset.py:16(__init__)
10    0.000    0.000    0.000    0.000 _weakrefset.py:52(_commit_removals)
```
**So if we have a C weakrefset than we could reduce time.**

So I started with the implementation,(its not complete yet...refcnts still need to be fixed)...check it [here](https://github.com/bhavishyagopesh/gsoc_2017/blob/master/python_startup_time/weakrefsetmodule.c).


## Acknowledgements
- Inada naoki -> For eveything in this blogpost.
- Victor Steinner -> For occassionally answering c-api doubts(on irc) and sharing his views on python-performance in general.
- `xxmodule.c` -> For providing the template to write extension modules in C :)
