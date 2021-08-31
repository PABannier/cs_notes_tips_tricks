# Unix tips

This file is just a non-exhaustive list of UNIX commands I've learnt over the
weeks. 

## A useful pager

```
more + <filename>
```

Allows you to see the content of a file in the terminal, you can browse it, go
to the top and/or bottom of the file. Very useful to inspect a csv file on a 
remote server for instance.


## Killing processes

Cmd + Z makes you quit a running process. Very useful to exit a buggy script
in a shell. However, the process is not killed and the memory it takes is not
released. To avoid memory leaks, run

```
kill %
```

This will kill all running processes in your shell window.


## Accessing disks

```
df -h
```

Prints a list of all available disks on your computer.


## Monitoring the running processes on a machine

When working with a remote machine (using SSH for instance), you usually want to
track the CPU and memory load of the server/machine (especially for shared computers).

To do so, in one tab of your shell, run your script. In another tab, run

```
htop
```

It gives you summary statistics about the server load. 

Note: htop might not be available on your computer, use `top` instead.


## Downloading a file from your computer

```
Wget <LINK>
```

Allows you to download a file at the specified location. Very useful when you want to
download a file from a windowless server.


## Symbolic links

```
ln
```

Allows you to create a symbolic link. A symoblic link at a location points to a specific
file. When you remove the symbolic link, you don't remove the underlying file.

```
ls -l
```

Lists all the symbolic links in the current working directory.


## Finding a previous command

To find a specific command, use

```
history | grep git
```

`history` displays the history of the commands typed by the user. `grep` is used
to search across a list. Here the above command searches the previous commands containing
git.


## Finding a file

```
find <DIRECTORY> <CRITERION>
```

Allows to find files mentioning the criterion.

## Misc shortcuts

Cmd + A: set the cursor at the beginning of the line
Cmd + K: erase the line
