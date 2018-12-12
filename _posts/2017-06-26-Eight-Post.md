---
layout: post
title: Another set of ideas(probably for second evaluation)
---

NOTE:*For system specs see [here](https://bhavishyagopesh.github.io/Second-Post/)*

## Possibilties to improve ABCs
1. My [earlier](https://bhavishyagopesh.github.io/Sixth-Post/) post proposed for a *C weakset*(and its structurally complete) as a way to  improve performance of **ABCs(Abstract base classes)** and it was discussed on core-mentorship ML.It was proposed that this might increase complexity and also would need a corresponding improvement in test suite.So it still needs to be discussed.

But **Inada** also made other suggestions to improve "ABCs":

Heavy import times Eg case-> [**typing**](https://gist.github.com/methane/a75ff64289f0aad7e3690d0c8884e9e7) module:(py3 provides special syntax(->) so as to Third party packages(like *mypy*) can do type checks):
He pointed out that [this](https://github.com/python/cpython/blob/5ff7132313eb651107b179d20218dfe5d4e47f13/Lib/abc.py#L134-L143)
part of code is the most slow  as text getattr(value, "__isabstractmethod__", False) is called for *all* class attributes of ABCs (including subclass of ABCs).Now  When the value is not abstractmethod, AttributeError is raised and cleared internally, getattr uses method cache (via PyType_Lookup), but `__isabstractmethod__` is mostly in instance __dict__.  So checking method cache is mostly useless efforts."


 2. But **Victor Stinner** came up with the suggestion of stripping annotations(in specific example of typings module) in cached *.pyc*(this way annotations would remain in code(and static type checking could is unaffected) but not in cached files thus decreasing time.)

 Also this could be done in harmony with `fat optimizer` project(see PEP [509](https://www.python.org/dev/peps/pep-0509/),[510](https://www.python.org/dev/peps/pep-0510/),[511](https://www.python.org/dev/peps/pep-0511/)).

 About `fat python`: This project aims to provide compiler optimizations (like constant propagation,constant folding etc) when and where possible, This is ensured using **guards**(see PEP510).Presently there are quite a few things(the [TODo list](https://fatoptimizer.readthedocs.io/en/latest/todo.html#todo)) that needs to be done in `fat-python`.I intend to take them up and that way I could expand to other benchmarks too.

 Links:
 - [fatoptimizer](https://fatoptimizer.readthedocs.io/en/latest/)
 - [fat module](https://fatoptimizer.readthedocs.io/en/latest/fat.html#fat)
