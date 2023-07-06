# Research tips

This note was initially updated on a local disk during an internship at INRIA
Saclay in the Parietal team. Since those pieces of advice are rather general,
I decided to keep them in mind for any research or CS-related endeavours.

## ALWAYS start simple

- When creating a product...
- When working on a new technique / algorithm...
- When testing a new method...
- When learning a new piece of technology...
- And for virtually anything...

Always start by a simple experiment. Ideally, you don't want to encapsulate any piece
of code in a function or in a class.

```
# A good setup for fast iteration

import numpy as np
# Import other libs

if __name__ == "__main__":
    # Do your experiments (no function, no class)
```

The reason for that is that you must focus on the task at hand, which is **understanding**
how the thing you are working on functions. You want to be able to quickly access
any parameter or line of code. That allows you to iterate quickly.

In short, **make it run, make it nice, make it fast**. Too often, we are tempted to
make our code very fancy right from the beginning, thinking that a well-organized codebase
will allow us to easily understand / learn new stuff. In reality, switching between files
is cumbersome and diverts you from your true objective: learning fast.

This is by far the most important tip.

## Don't work on more than 2 or 3 sub-projects at a time

This one is pretty self-explanatory.


## Be ruthless on testing

Always write unit and integration tests. It is tempting to write easy tests at the first time
but you will regret it sooner or later. Always write hard tests and if you relax your test,
make sure to know why you NEED to relax it.

**When a test is not passing, isolate it in a script and play with it with ipdb to understand the mistake**.


## Jupyter Notebook is good, Ipython is better

Jupyter Notebook is good for interactive data visualization, and for EDA. However, it is a
terrible habit to write code in Jupyter Notebook, that takes you away from solid and
professional software engineering practices.

A good compromise is **Ipython**. Ipython is simply an interactive Python shell to run your
Python scripts. It allows you to run your script, interactively debug and play with in-memory
variables. A typical setup would be:

1. Write your script on VSCode
2. Run on Ipython your script by running:

```
    %run <NAME_OF_SCRIPT>.py
```

Some quick commands:

- Cmd + Z: kill the process (then write `kill %` to clean all running processes).
- Cmd + C: quit the process
- Cmd + D: disconnect from a server in Shell (SSH for instance).


## Use iTerm2

Much more customizable shell, that works hand-in-hand with zsh.


## Use a Matplotlib visualization kernel

```
ipython --matplotlib=qt
```

When using ipython with matplotlib, displaying a figure might interrupt the execution
of your script. To show a figure while your program keeps running, use the option
aforementionned.


## Use joblib cached memory to speed up code

When your script keeps loading the same data or keep generating the same data in order
to do calculations with it, cache this data using joblib using:

```
from joblib import Memory

mem = Memory(".")  # Stores in the current directory

@mem.cache
def foo():
    # code...
```

## Speed up your script by parallelizing execution

The only advice to keep in mind is to always parallelize at the highest level possible.
For instance, if you are solving a bunch of M/EEG inverse problems, you want to dedicate
one thread to one inverse problem. Do not parallelize the optimization algorithm at a
lower level of your script, you won't get as much speed up.

Use the loky backend of Joblib with:

```
from joblib import parallel_backend
from joblib import Parallel, delayed

INNER_MAX_NUM_THREDS = 1
N_JOBS = 4

with parallel_backend("loky", inner_max_num_threads=INNER_MAX_NUM_THREADS):
    Parallel(N_JOBS)(
        delayed(foo)(*args)
        for element in elements
    )
```

## An interactive debugger

When working with Python, you usually want to debug a program to solve issues in your
scripts. Use the `ipdb` module for that. A simple use case is

```
# SOME CODE
import ipdb; ipdb.set_trace()
# SOME CODE
```

When running the script from ipython, the execution will stop at the desired location
and let you play with the in-memory variables.

Besides, when a program crashes in ipython, you can still run in the next cell

```
%debug
```

which will setup a debugguer at the location right before the crash.


## Interactive debugger for testing

```
pytest --pdb
```

Allows you to interactively debug your tests.

Very useful when you combine multiple assertions in one test.


## Use Mamba as a package manager

Mamba is a Python package and environment manager similar to Conda, but written in
C++. It has the advantages to be much faster (much less time spent on resolving the
environment for instance).


## Use Pathlib instead of os.path

Pathlib is much more flexible and should be priviledged over `os.path`.


## Linting your Python code

`black` is a great code formatter for Python, as it can automatically format your
code on save. However, a cleaner and more wide-spread alternative in open-source
projects is `flake`.

If a `flake` config file is available, run:

```
make flake
```

Otherwise, run

```
flake8
```


## Debugging Pydocstyle

Pydocstyle is useful for litterate programming. It allows you to generate a documention
from docstrings. When a config file is available, run

```
make pydocstyle
```


## Profiling your code in Python

To profile your code line by line, use `kernprof`. At the time of writing, beware
that version 3.2.0 of `line_profiler` is broken. Install 3.1.0.

A useful resource for profiling without `kernprof`:
https://lothiraldan.github.io/2018-02-18-python-line-profiler-without-magic/ .


## Github plugin for VSCode

Very classic. Useful to add files to your commit.
Note to self: add files with VSCode but do all the git commands directly in shell.


## SSH to a server directly in VSCode

Look for the Extension Remote SSH with VSCode. This allows you to directly SSH
to a remote server and develop on a remote machine.

A more flexible solution is to use Rsync which allows you when generating data
to have multiple directories on a remote and local machines synced up automatically.
It is particularly useful when generating figures.


## Inspecting your logs post-mortem with `tee`

When launching a script on a remote server, you might find later on that your pod has
crashed and the process you launched was not able to complete. When faced with this situation,
you'd like to inspect post-mortem what is the cause of the crash of the server? Is it
your script specifically? Ideally, you'd like to access the stacktrace of our programs
or the logs.

One way to do that is to intercept `stdout` and write it into a file with the POSIX
utility `tee`. This utility allows you to capture the `stdout` of your process and write
it into a file on disk, something very handy if your script does not already write into a
log file. Here is basic example below:

```bash
lint program.c | tee program.lint`
```

This simple command allows to lint a C file and to pipe the output of the linting process
into a log file called `program.lint` using `tee`.
