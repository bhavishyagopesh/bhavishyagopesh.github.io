---
layout: post
title: On Kcachegrind
---

NOTE:*For system specs see [here](https://bhavishyagopesh.github.io/Second-Post/)*

### About Kcachegrind

As I was analysing the `performance` module, I found this amazing tool for visualization of `function-calls`
.Here's how you use it:

```text
$ python -m cProfile -o myscript.cprof myscript.py
$ pyprof2calltree -k -i myscript.cprof
```
It opens up in a new Kcachegrind window.Here are some sample screenshots:

Go to [this](https://github.com/bhavishyagopesh/gsoc_2017/tree/master/kcachegrind) repo to see some screenshots.
