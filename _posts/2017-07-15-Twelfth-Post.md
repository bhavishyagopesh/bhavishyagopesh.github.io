---
layout: post
title: Analysis of `sqlite_synth` benchmark.
---

NOTE:*For system specs see [here](https://bhavishyagopesh.github.io/Second-Post/)*

### Comparison b/w py2 and py3 on *sqlite_synth*:

*Obtained using perf module i.e. `perf module([here](https://bhavishyagopesh.github.io/Third-Post/))`*

- py2 -> 6.70 us +- 0.11 us
- py3 -> 8.49 us +- 0.16 us: **1.27x slower (+27%)**

### About the benchmark:
This benchmark measures the **CFFI(C Foreign Function Interface) performance** by going back and forth between `sqlite` and `python`.

## Analysis:

- Call to `Lib/sqlite3` module is **common** to both py2 and py3.

Execpt call to `ABC` and use of `byte-strings`:

```text
py2                                          |            py3                                            
  import collections                         |           import collections.abc                              
  Binary = memoryview		                 |            Binary = buffer                                    
  collections.abc.Sequence.register(Row)	 |           collections.Sequence.register(Row)                  
```

ABCs,as being discussed from start, might be the reason for slowdown(as pointed it [here](https://bhavishyagopesh.github.io/Eight-Post/)) and also **their are bytes string** used(again time is  spent on that)

- Call to `Modules/_sqlite` :

In `connection.c`

May be the code that manages the GIL:
```text
static void _trace_callback(void* user_arg, const char* statement_string)		
	{		
	    PyObject *py_statement = NULL;		
	    PyObject *ret = NULL;		

	#ifdef WITH_THREAD		
	    PyGILState_STATE gilstate;		

	    gilstate = PyGILState_Ensure();		
	#endif		
	    py_statement = PyUnicode_DecodeUTF8(statement_string,		
	            strlen(statement_string), "replace");		
	    if (py_statement) {		
	        ret = PyObject_CallFunctionObjArgs((PyObject*)user_arg, py_statement, NULL);		
	        Py_DECREF(py_statement);		
	    }		

	    if (ret) {		
	        Py_DECREF(ret);		
	    } else {		
	        if (_enable_callback_tracebacks) {		
	            PyErr_Print();		
	        } else {		
	            PyErr_Clear();		
	        }		
	    }		

	#ifdef WITH_THREAD		
	    PyGILState_Release(gilstate);		
	#endif		
}
```

```text
static PyObject* pysqlite_connection_set_trace_callback(pysqlite_Connection* self, PyObject* args, PyObject* kwargs)		
	{		
	    PyObject* trace_callback;		

	    static char *kwlist[] = { "trace_callback", NULL };		

	    if (!pysqlite_check_thread(self) || !pysqlite_check_connection(self)) {		
	        return NULL;		
	    }		

	    if (!PyArg_ParseTupleAndKeywords(args, kwargs, "O:set_trace_callback",		
	                                      kwlist, &trace_callback)) {		
	        return NULL;		
	    }		

	    if (trace_callback == Py_None) {		
	        /* None clears the trace callback previously set */		
	        sqlite3_trace(self->db, 0, (void*)0);		
	    } else {		
	        if (PyDict_SetItem(self->function_pinboard, trace_callback, Py_None) == -1)		
	            return NULL;		
	        sqlite3_trace(self->db, _trace_callback, trace_callback);		
	    }		

	    Py_RETURN_NONE;		
}
```
But I'm not completely sure.May be I would insert `time` bw these regions of code to find anything useful.
You can see the *vmprof* profiles [here](https://github.com/bhavishyagopesh/gsoc_2017/tree/master/vmprof) and *opcodes* [here](https://github.com/bhavishyagopesh/gsoc_2017/tree/master/opcodes).But they are of not much help in this case.
