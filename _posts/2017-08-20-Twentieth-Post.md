---
layout: post
title: Benchmark for Concurrency
---

### Benchmark for concurrency;
So there are two primitives for doing things concurrently in *python* namely `Threading` and `Multiprocessing` module.
NOTE: **`Threading` actually is restricted by GIL and there is no real Concurrency.**
[Here's](https://github.com/bhavishyagopesh/gsoc_2017/blob/master/new_benchmarks/bm_concurrent.py) the code for concurrent Benchmark.

### Statistics:
```bash
 py2:
 number_crunching: Mean +- std dev: 35.9 ms +- 1.0 ms

 py3:
 number_crunching: Mean +- std dev: 64.1 ms +- 4.1 ms

 And after turning *x to Long *in py2:lt text
 number_crunching: Mean +- std dev: 168 ms +- 11 ms
```

### Method Used:
The above script tries to benchmark "Concurrency" implemented using threading and multiprocessing module.Actually "threads" in cpython are restricted by "GIL",so it's not actually concurrent...On the other hand "multiprocessing" module creates whole different processes but there is substaintial cost involved in spawing a whole new process.
So the there is a trade-off involved which is evident as we increase "CRUNCH_NO" variable.

## Observations:
Interestingly Here py2 looks faster.(actually by a considerable margin)

Here are graphs that compare the two:

[py2](https://github.com/bhavishyagopesh/gsoc_2017/blob/master/graphs/py2-001.jpg)

[py3](https://github.com/bhavishyagopesh/gsoc_2017/blob/master/graphs/py3-001.jpg)

### Benchmark for Threading-objects benchmark:
This bm is **not of much** practical use but tries to measure the cost of creating common `threading` oblects.

### Statistics:
```bash
python3 bm_threading.py
.....................
basic: Mean +- std dev: 14.4 us +- 0.4 us
.....................
condition: Mean +- std dev: 18.1 ns +- 1.1 ns
.....................
lock: Mean +- std dev: 18.1 ns +- 0.9 ns
.....................
rlock: Mean +- std dev: 19.4 ns +- 1.6 ns
.....................
semaphore: Mean +- std dev: 298 ns +- 56 ns
.....................
timer: Mean +- std dev: 6.74 us +- 0.65 us



python2 bm_threading.py
.....................
basic: Mean +- std dev: 20.5 us +- 1.1 us
.....................
condition: Mean +- std dev: 148 ns +- 2 ns
.....................
lock: Mean +- std dev: 21.1 ns +- 0.7 ns
.....................
rlock: Mean +- std dev: 151 ns +- 3 ns
.....................
semaphore: Mean +- std dev: 336 ns +- 4 ns
.....................
timer: Mean +- std dev: 12.5 us +- 0.7 us
```
