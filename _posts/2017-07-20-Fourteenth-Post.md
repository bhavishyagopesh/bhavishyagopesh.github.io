---
layout: post
title: Startup-time revisited...
---

NOTE:
- *For system specs see [here](https://bhavishyagopesh.github.io/Second-Post/)*
- These would be a series of posts...as it is realised that the reason for regressions **are very much common across bms.**

NOTE: *For better visualization of what is described below install Kcahegrind and than do
`kcachegrind file-name`  and for files look [here](https://github.com/bhavishyagopesh/gsoc_2017/tree/master/kcachegrind) (the ones with * \*callgrind suffix\* *)*

## The different way of initialisation:
NOTE:All percentages are of **corresponding startup-time** which are:(obtained using `perf` module [here](https://bhavishyagopesh.github.io/Third-Post/))

| Benchmark               | py2      | py3                           |
+-------------------------+----------+-------------------------------+
| python_startup          | 19.2 ms  | 42.3 ms: 2.21x slower (+121%) |
+-------------------------+----------+-------------------------------+


For both py2(**always 2.7**) & py3(**always 3.7**) start with `Py_Main` but the initialisation is different in both->
)
1.    py2:
     - `Py_Initialize()`[5.69%]->`Py_InitializeEx` [$](https://gist.github.com/bhavishyagopesh/376b52ffb71e6b639872eedc5c07b5a9)
      py3:
     - `_Py_InitializeMainInterpreter`[9.98%]-> `initsite`[6.07%]\(there are calls to other functions and error checking that takes rest of time   [$](https://gist.github.com/bhavishyagopesh/0531fe5d0271491965c6ab5f68495ca9) \)

     - `Py_FinalizeEx`[6.25%]\(This function does the cleanup job by calling `PyImport_Cleanup` (which empties the module table it). *The way of cleaning things is different from py2*  \)


2.   py2:
    - `PyRun_AnyFileExFlags`[91.81%]\( It can run any-interactive console or normal file and in our case it simply calls `PyRun_FileExFlags` eventually\) it than calls `run_mod`      [$](https://gist.github.com/bhavishyagopesh/91017a33b93a92a7269dd6838e186672)(which calls `PyParser_ASTFromFile`[34.67%] and `PyEval_EvalFrameEx`[7.34%])

    py3:
    - `run_file`[77.42%] than calls `PyRun_AnyFileExFlags` and finally `run_mod`[$](https://gist.github.com/bhavishyagopesh/0dd58cdf9171ea33f7b7872a513b64a7) (here it uses **Py_Object** as compared to py2's **const char\*** --*optimization oppurtunity maybe* )and now the functions involving `AST` and `encoding` are called.

Note:
1. I have added `gists`[$] for relevant function declaration.
2. Remaining steps will be covered in next post.
