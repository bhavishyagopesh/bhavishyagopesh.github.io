---
layout: post
title: Lazy-loading as an optimization possibility!!!
---

NOTE:*For system specs see [here](https://bhavishyagopesh.github.io/Second-Post/)*

## Use Lazy_Loader for import of modules during "startup", so as to prevent import of module which are not necessary and explore the possibility of possible decrease in startup time.

### What is lazy-loading?

What lazy-loading does is that, it loads a module only when **it's attributes** are acccessed.This could be very important for applications that import a hell lot of stuff at start and thus could help lower their startup time.

Following links describe the status of imports in python presently:
- [PEP 302](https://www.python.org/dev/peps/pep-0302/) first to propose `import_hooks`
- But [importlib](https://docs.python.org/3/library/importlib.html#importlib.__import__) much better interface to write custom modules

And following for lazy-loading:
- Proposal of `lazy import` keyword [here](https://mail.python.org/pipermail/python-ideas/2008-October/002193.html)
- Also `importlib.util` provides `LazyLoader` class for creating "lazy" instances [here](https://docs.python.org/3/library/importlib.html#importlib.__import__)

### Effect on startup time

Following two possibilties were to be explored:-
1) If *lazy-loading* could actually be done so as to decrease the "startup time" by **restricting import only to modules whose attributes are actually acccessed.**
2) There might be some **implicit imports** which do not come up, so atleast detect them.

I wrote a *[lazy_import_module](https://github.com/bhavishyagopesh/gsoc_2017/blob/master/python_startup_time/lazy_loader.py)* function and [here's](https://github.com/bhavishyagopesh/gsoc_2017/blob/master/python_startup_time/lazy_vs_nonlazy) its comparision with a *[nonlazy_import_module](https://github.com/bhavishyagopesh/gsoc_2017/blob/master/python_startup_time/nonlazy_loader.py)*.
