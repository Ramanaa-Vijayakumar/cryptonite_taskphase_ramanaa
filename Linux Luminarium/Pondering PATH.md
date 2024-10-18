# Pondering PATH

Thus far, you have invoked commands in several ways:

Through an absolute path (e.g., `/challenge/run`).
Through a relative path (e.g., `./run`).
Through a bare command name (e.g., `ls`).

The first two cases, the absolute and the relative path case, are straightforward: the `run` file lives in the `/challenge` directory, and both cases refer to it (provided, of course, that the relative path is invoked with a current working directory of `/challenge`). But what about the last one? Where is the `ls` program located? How does the shell know to search for it there?

[What is PATH?](https://www.linfo.org/path_env_var.html)

---

## The PATH Variable

It turns out that the answer to "How does the shell find `ls`?" is fairly simple. There is a special shell variable, called `PATH`, that stores a bunch of directory paths in which the shell will search for programs corresponding to commands. If you blank out the variable, things go badly:

```
hacker@dojo:~$ ls
Desktop    Downloads  Pictures  Templates
Documents  Music      Public    Videos
hacker@dojo:~$ PATH=""
hacker@dojo:~$ ls
bash: ls: No such file or directory
hacker@dojo:~$
```

Without a PATH, bash cannot find the `ls` command.

### CTF

```
hacker@path~the-path-variable:~$ PATH=""
hacker@path~the-path-variable:~$ /challenge/run
Trying to remove /flag...
/challenge/run: line 4: rm: No such file or directory
The flag is still there! I might as well give it to you!
pwn.college{QGXKl_n-klBntz1jgw11sMs_AZX.dZzNwUDL5kTO0czW}
```

---

## Setting PATH

Okay, so things break when you blank out `PATH`. But what about doing something *useful* with `PATH`?

Let's explore how we would, for example, add a new directory of programs to our command repertoire. Recall that `PATH` stores a list of directories to find commands in and, for commands in nonstandard places, we must typically execute them via their path:

```
hacker@dojo:~$ ls /home/hacker/scripts
goodscript	badscript	okayscript
hacker@dojo:~$ goodscript
bash: goodscript: command not found
hacker@dojo:~$ /home/hacker/scripts/goodscript
YEAH! This is the best script!
hacker@dojo:~l
```

If you maintain useful scripts that you want to be able to launch by bare name, this is annoying. However, by adding directories to or replacing directories in this list, you can expose these programs to be launched using their bare name! For example:

```
hacker@dojo:~$ PATH=/home/hacker/scripts
hacker@dojo:~$ goodscript
YEAH! This is the best script!
hacker@dojo:~$
```

### CTF

```
hacker@path~setting-path:~$ PATH=/challenge/more_commands
hacker@path~setting-path:~$ /challenge/run win
Invoking 'win'....
Congratulations! You properly set the flag and 'win' has launched!
pwn.college{grDrK5ilhjwtu0LxURTOuk0Kjt3.dVzNyUDL5kTO0czW}
```

---

## Adding Commands

### CTF

```
hacker@path~adding-commands:~$ nano win
hacker@path~adding-commands:~$ chmod u=rwx win
hacker@path~adding-commands:~$ export PATH=/home/hacker:$PATH
hacker@path~adding-commands:~$ /challenge/run
Invoking 'win'....
pwn.college{ob7Q-Eg5Dq7_IZvW8Iz6IxPae4u.dZzNyUDL5kTO0czW}
```

> SENSAI
When setting the `PATH`, it's crucial to ensure you include both your custom directory and the existing directories. This way, your `win` script can be found, and you won't lose access to essential commands like `cat`.

> Here's a general approach to modifying the `PATH`:

> `export PATH=/path/to/your/script:$PATH`

> Replace `/path/to/your/script` with the actual directory where your `win` script is located. This command prepends your directory to the existing `PATH`, ensuring both your script and other commands are accessible.

> Try setting your `PATH` this way and see if it helps! Let me know how it goes.

---

## Hijacking Commands

### CTF

```
hacker@path~hijacking-commands:~$ nano rm
hacker@path~hijacking-commands:~$ ls -l rm
-rw-r--r-- 1 hacker hacker 10 Oct 18 06:06 rm
hacker@path~hijacking-commands:~$ chmod a=rwx rm
hacker@path~hijacking-commands:~$ pwd
/home/hacker
hacker@path~hijacking-commands:~$ export PATH=/home/hacker:$PATH
hacker@path~hijacking-commands:~$ /challenge/run
Trying to remove /flag...
Found 'rm' command at /home/hacker/rm. Executing!
pwn.college{8Ia0OyJM3hP8w1kBxs6KMVMUI1N.ddzNyUDL5kTO0czW}
```

---


