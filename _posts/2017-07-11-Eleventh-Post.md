---
layout: post
title: Moving on as Pickle/Unpickle pure-python benchmark has low practical value.
---

NOTE:*For system specs see [here](https://bhavishyagopesh.github.io/Second-Post/)*

### Pickle/Unpickle pure-python:

[Pickling](https://docs.python.org/3/library/pickle.html): Serialization into byte code(using some protocols, see the links for official docs.) and Unpickling is the opposite.

Py3 is better at py2 at most pickling bms except the one's involving pure-python,here:

*Obtained using perf module i.e. `perf module([here](https://bhavishyagopesh.github.io/Third-Post/))`*

- unpickle_pure_python: 384 us +- 8 us -> 670 us +- 11 us: 1.74x slower (+74%)
- pickle_pure_python: 847 us +- 14 us -> 974 us +- 14 us: 1.15x slower (+15%)

Now py2 builds upon the marshal module(which is a primitive alternative to pickle) whereas py3 uses
codecs, _compat_pickle,... And I thought may be the import of different modules could be responsible for greater import time.And I talked to Victor and Inada about this, and they both said that pure-python pickle is not used commonly and **_pickle.c** is used more often.

Victor even has proposed to remove pure-python from the list of benchmarks.([here](https://mail.python.org/pipermail/speed/2017-April/000554.html)).But the [cloudpickle](https://github.com/cloudpipe/cloudpickle) uses it as `_pickle` **does not allow overriding the "save_global" method to support saving more objects(such as closures, etc.)** which cloudpickle requires.

So I would move on to some-other benchmark(probably sqlite_synth).
