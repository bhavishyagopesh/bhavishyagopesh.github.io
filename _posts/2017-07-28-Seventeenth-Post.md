---
layout: post
title: A dedicated zip-archive for commonly imported modules
---

## Creating a zip-archive:

I wrote a [`python-script`](https://github.com/bhavishyagopesh/gsoc_2017/blob/master/zip-archive/with_zip2/common_module_archive.py) which created a zip-archive containing some common modules (which are imported at startup),like:

- \_weakrefset
- abc
- encodings
- codecs
- io
- os

And to get complete isolation I ran those two(with_zip and without_zip) inside **docker containers** [here](https://github.com/bhavishyagopesh/gsoc_2017/tree/master/zip-archive)

And the performance of `with_zip` is slightly better than `without_zip`. Here are some stats:

```bash
#Without adding zip-archive


$ python -m site
sys.path = [
    '/',
    '/usr/local/lib/python27.zip',
    '/usr/local/lib/python2.7',
    '/usr/local/lib/python2.7/lib-dynload',
    '/usr/local/lib/python2.7/site-packages',
]
USER_BASE: '/root/.local' (doesn't exist)
USER_SITE: '/root/.local/lib/python2.7/site-packages' (doesn't exist)
ENABLE_USER_SITE: True

$ python bm_python_startup.py
.....................
python_startup: Mean +- std dev: 10.1 ms +- 0.5 ms

```


```bash
#With zip-archive


$ python common_module_archive.py

$ export PYTHONPATH=common_module_archive.zip

$ python -m site
sys.path = [
    '/',
    '/common_module_archive.zip',
    '/usr/local/lib/python27.zip',
    '/usr/local/lib/python2.7',
    '/usr/local/lib/python2.7/lib-dynload',
    '/usr/local/lib/python2.7/site-packages',
]
USER_BASE: '/root/.local' (doesn't exist)
USER_SITE: '/root/.local/lib/python2.7/site-packages' (doesn't exist)
ENABLE_USER_SITE: True


$ python bm_python_startup.py
.....................
python_startup: Mean +- std dev: 9.86 ms +- 0.1 ms

```

NOTE:
1. I ran the benchmark  many times and each run resulted in better(or equal) time for **zipped** version.
2. This might **not** bring huge benefits because in writing a custom-importer we are already using import of **some common modules** and also python by itself adds a .zip of library in `sys.path`.
3. More instructions on reproducing this can be found [here](https://github.com/bhavishyagopesh/gsoc_2017/tree/master/zip-archive/with_zip2)
