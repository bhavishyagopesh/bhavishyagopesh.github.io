---
layout: post
title: Analysis of `crypto_pyaes` benchmark.
---

NOTE:*For system specs see [here](https://bhavishyagopesh.github.io/Second-Post/)*

### Comparison b/w py2 and py3 on *crypto_pyaes*:

*Obtained using perf module i.e. `perf module([here](https://bhavishyagopesh.github.io/Third-Post/))`*

- py2 -> 158 ms +- 1 ms
- py3 -> 199 ms +- 2 ms: **1.26x slower (+26%)**

### About the benchmark:
This benchmark is based on `pyaes` modules and uses the *CTR* mode of operation.

## Analysis:

 Following might be the reasons for regression: (NOTE: Here both py2 and py3 are using the same `pyaes` module.)

 1. Import time:
- Here too, **import-time** is majorly responsible for regression.Analysis of imports(obtained using `-v` flag [here](https://github.com/bhavishyagopesh/gsoc_2017/tree/master/python_verbose)) supports this fact.These modules specifically are extra(as compared to py2):

- weakref(Also/Hence `weakrefset`)
- collections.abc
- builtins
- \_imp   (for imports_)           
- codecs
- encodings(NOTE: the `pyaes` module onlyuses **byte-strings**)
- reprlib
- enum
- selectors
- tokenize
- bisect(\_bisect)
- \_compression

2. Py2 uses **StringIO(cString)** whereas py3 use *io text wrapper*.Hence slow.

Here are [opcodes](https://github.com/bhavishyagopesh/gsoc_2017/tree/master/opcodes) and [cProfiles](https://github.com/bhavishyagopesh/gsoc_2017/tree/master/cProfiles).

**But as with the `sqlite_synth` benchmark they are almost same and do not provide any extra insights**
