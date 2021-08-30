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

