---
layout: post
title: Benchmark for math and smtplib module 
---

### Benchmark for math module:
[Here's](https://github.com/bhavishyagopesh/gsoc_2017/blob/master/new_benchmarks/bm_math.py) the code for the Benchmark for math module.

### Statistics:
```bash
python2 bm_math.py
.....................
basic_math: Mean +- std dev: 30.8 ms +- 0.2 ms

python3 bm_math.py
.....................
basic_math: Mean +- std dev: 31.3 ms +- 1.2 ms
```

### Observations:
- The math benchmark's regression is within standard deviation, so it's not that important.

###  Benchmark for smtplib module:
[Here's](https://github.com/bhavishyagopesh/gsoc_2017/blob/master/new_benchmarks/bm_smtplib.py) the code for the Benchmark for smtplib module.

### Statistics:
```bash
python3 bm_smtplib.py
.....................
smtplib: Mean +- std dev: 13.2 ms +- 22.8 us


python2 bm_smtplib.py
.....................
smtplib: Mean +- std dev: 9.66 ms +- 32.52 us
```
### Observations:
- The smtplib benchmark regresses significantly

