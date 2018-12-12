---
layout: post
title:  New benchmark modules --zlib
---

### New benchmarks -- zlib module

On advice from my mentor I wrote explicit benchmarks for modules that don't have them presently.First in the series is the `zlib` module.Here's the [code](https://github.com/bhavishyagopesh/gsoc_2017/blob/master/new_benchmarks/bm_zlib.py).

### Statistics from bm:
```bash
1)SIZE=1000

python3 bm_zlib.py
.....................
compress: Mean +- std dev: 16.2 ms +- 0.8 ms
.....................
decompress: Mean +- std dev: 3.72 ms +- 0.36 ms

python2 bm_zlib.py
.....................
compress: Mean +- std dev: 16.2 ms +- 0.6 ms
.....................
decompress: Mean +- std dev: 3.65 ms +- 0.15 ms

2)SIZE=10000000

python3 bm_zlib.py
.....................
compress: Mean +- std dev: 161 ms +- 3 ms
.....................
decompress: Mean +- std dev: 33.3 ms +- 1.1 ms

python2 bm_zlib.py
.....................
compress: Mean +- std dev: 159 ms +- 1 ms
.....................
decompress: Mean +- std dev: 32.6 ms +- 0.4 ms
```

### Observations:
```text
py3                                                    |  py2
                                                       |
0.578 {method 'read' of '_io.TextIOWrapper' objects}   | 0.566 {method 'read' of 'file' objects}
.007 decimal.py                                        | 0.001 decimal.py
 0.003 threading.py                                    |          
 0.001 enum.py                                         |
 0.002 signal.py                                       |
 0.002 decoder.py                                      |
 0.002 tokenize.py                                     |   
 0.001 sre_compile.py |
```
- The zlib benchmarks becomes important as "the length of binary string increases".
