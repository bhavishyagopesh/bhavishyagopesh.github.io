---
layout: post
title: No benefit from Lazy-Loading
---

NOTE:*For system specs see [here](https://bhavishyagopesh.github.io/Second-Post/)*

### Hitting the wall with Lazy-Loading...

This is a *short entry* for making a note that lazy-loading(mentioned in this [post](https://bhavishyagopesh.github.io/Seventh-Post/)) **won't be** useful in improving the startup time.
On the contrary it increased the python-startup-time.Here's some statistics:

- `Original python -> "python -c "python -c '4'  0.01s user 0.00s system 99% cpu 0.017 total`

As with more replacements:
- `Modified version -> "./python -c '4'  0.02s user 0.00s system 98% cpu 0.024 total"
                       "./python -c '4'  0.03s user 0.00s system 99% cpu 0.026 total"
                       "./python -c '4'  0.03s user 0.00s system 99% cpu 0.032 total "
  `
### NOTE:

- This by no means shows that Lazy-Loading is not useful, actually it's a kinda essnetial for "real-world applications" that do a lot of early import.For example bazaar does this in production [see [bzr](https://sourceforge.net/p/adempiere/discussion/610546/thread/bba7629f/)].
- I'm now working on fat-python(I have stopped now as Victor suggested).
