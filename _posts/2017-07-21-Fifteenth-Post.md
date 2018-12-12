---
layout: post
title: Summary and further actions...
---

### Regressed benchmarks:

| **Benchmark**           | **py2**     |  **py3**   | **Times-slow**        |
| :-------                | :-------    | -------:   | -------:              |
| python_startup_no_site  |    9.42 ms  |   26.0 ms  |   2.76x slower (+176%) |
| python_startup          |    19.2 ms  |   42.3 ms  |   2.21x slower (+121%) |
| spectral_norm           |    194 ms   |   259 ms   |   2.20x slower (+120%) |
| sqlite_synth            |    6.70 us  |   8.49 us  |   1.27x slower (+27%)  |
| crypto_pyaes            |    158 ms   |   199 ms   |   1.26x slower (+26%)  |
| xml_etree_parse         |    193 ms   |   242 ms   |   1.25x slower (+25%)  |
| xml_etree_iterparse     |    154 ms   |   179 ms   |   1.16x slower (+16%)  |
| go                      |    439 ms   |   493  ms  |   1.12x slower (+12%)  |

NOTE:
1. `logging_simple` and `logging_format` were fixed by [this PR](https://github.com/python/performance/pull/27)
2. Also bms involving `pure-python pickle` are not considered because of their low **practical value**.
3. Also to reproduce these(all benchmarks) you may consider these [scripts and Dockerfiles](https://github.com/bhavishyagopesh/gsoc_2017/tree/master/reproducible_instances/startup_time/basic_comparision) but you can also reproduce them [without docker](https://bhavishyagopesh.github.io/Third-Post/)

### Idea of parallelizing marshalling :

If we could somehow paralleize `marshalling` and thus **"loading"(not "executing")** than it could speedup import and henceforth **"startup-time"**.But it **won't** improve things drastically as **loading is a small fraction of execution time**
Eg:-for complex module like "typings" -> "29x" greater but for smaller ones like "ABC"-> "4x" greater.

Here are [dockerfiles](https://github.com/bhavishyagopesh/gsoc_2017/tree/master/reproducible_instances/startup_time/load_and_exec) to reproduce above benchmark.
