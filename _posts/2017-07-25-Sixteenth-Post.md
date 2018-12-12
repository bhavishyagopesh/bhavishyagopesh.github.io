---
layout: post
title: Plan for Next Evaluation...
---

## New Experiments for sake of "startup-time":

- List of modules loaded `with` and `without sysconfig` are(on startup):

  With site->

  ```python

  >>> import sys
  >>> sorted(k for k,m in sys.modules.items() if m.__spec__ is not None and type(m.__spec__.loader).__name__=="SourceFileLoader")
['_collections_abc', '_sitebuiltins', '_sysconfigdata_m_linux_x86_64-linux-gnu', '_weakrefset', 'abc', 'codecs', 'encodings', 'encodings.aliases', 'encodings.latin_1', 'encodings.utf_8', 'genericpath', 'io', 'os', 'os.path', 'posixpath', 'rlcompleter', 'site', 'stat', 'sysconfig']

  ```

  Without site->

  ```python
  >>> import sys
  >>> sorted(k for k,m in sys.modules.items() if m.__spec__ is not None and type(m.__spec__.loader).__name__=="SourceFileLoader")
['_weakrefset', 'abc', 'codecs', 'encodings', 'encodings.aliases', 'encodings.latin_1', 'encodings.utf_8', 'io']
  
  ```

- As per "Nick Coughlan"'s suggestion I would be exploring following tests...as they directly effect  `python interpreter's`performance:

  - Pushing "commonly-imported" modules to a separate zip archive

  - `sys.modules` must be seeded with contents of that archive

  - freezing the import of those modules

  - Compiling the top-level standalone modules with Cython and using them as extension modules.

  NOTE: This would well extend past Second Evaluation.
