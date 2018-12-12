---
layout: post
title: Optimization in "logging" benchmark
---

NOTE:*For system specs see [here](https://bhavishyagopesh.github.io/Second-Post/)*

### py3 shows no regression on logging benchmark now...

So "logging" was one of the regressed benchmarks, here are some stats:

*Obtained using perf module i.e. `perf module([here](https://bhavishyagopesh.github.io/Third-Post/))`*

```text
|  logging_format          | 57.7 us  | 75.1 us: 1.30x slower (+30%)  |
+-------------------------+----------+-------------------------------+
| logging_silent           | 818 ns   | 1.00 us: 1.22x slower (+22%)  |
+-------------------------+----------+-------------------------------+
| logging_simple           | 46.2 us  | 70.0 us: 1.51x slower (+51%)  |
```

Earlier on in `bm_logging.py` used `Logger.warn()` inside loops but `warn()` is now
deprecated in favour of `warning()`.Also in py3 `Logger.warn()` calls `warning.warn()` [1] internally
while in py2 it's just an alias[2].

Thus I made a [PR](https://github.com/python/performance/pull/27) changing `warn to warning` and it showed significant improvement.Here goes the stats:

```text
For Logger.warn()

.....................
logging_format: Mean +- std dev: 28.7 us +- 0.8 us
.....................
logging_silent: Mean +- std dev: 679 ns +- 17 ns
.....................
logging_simple: Mean +- std dev: 24.5 us +- 1.1 us

For Logger.warning()

.....................
logging_format: Mean +- std dev: 24.4 us +- 0.9 us
.....................
logging_silent: Mean +- std dev: 699 ns +- 36 ns
.....................
logging_simple: Mean +- std dev: 20.7 us +- 0.9 us

```



- Acknowledgements: Victor(for reviewing and merging the PR:).
