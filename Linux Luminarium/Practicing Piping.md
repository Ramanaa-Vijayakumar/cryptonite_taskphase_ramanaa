# Practicing Piping

Every process in Linux has three initial, standard channels of communication:

- Standard Input is the channel through which the process takes input.
For example, your shell uses Standard Input to read the commands that you input.

- Standard Output is the channel through which processes output normal data, such as the flag when it is printed to you in previous challenges or the output of utilities such as `ls`.

- Standard Error is the channel through which processes output error details.
For example, if you mistype a command, the shell will output, over standard error, that this command does not exist.

Because these three channels are used so frequently in Linux, they are known by shorter names: `stdin`, `stdout`, `stderr`.

[Furthur Reading](https://web.archive.org/web/20220629044814/http://bencane.com:80/2012/04/16/unix-shell-the-art-of-io-redirection/)
[Pipes, Forks & Dups](http://www.rozmichelle.com/pipes-forks-dups/)

---

## Redirecting output

To redirect stdout to files. 
You can accomplish this with the `>` character, as so:

`hacker@dojo:~$ echo hi > asdf`

This will redirect the output of `echo hi` (which will be `hi`) to the file `asdf`.
You can then use a program such as `cat` to output this file:

`hacker@dojo:~$ cat asdf`
`hi`

### CTF

```
hacker@piping~redirecting-output:~$ echo PWN > COLLEGE
Correct! You successfully redirected 'PWN' to the file 'COLLEGE'! Here is your
flag:
pwn.college{IUEHKiAEpVlPd4OIlbCvSfYd6FC.dRjN1QDL5kTO0czW}
```

---

## Redirecting more output

Aside from redirecting the output of `echo`, you can, of course, redirect the output of any command.

### CTF

```
hacker@piping~redirecting-more-output:~$ /challenge/run > myflag
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge will check that output is redirected to a specific file path : myflag
[INFO] - the challenge will output a reward file if all the tests pass : /flag

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /flag file.

[TEST] You should have redirected my stdout to a file called myflag. Checking...

[PASS] The file at the other end of my stdout looks okay!
[PASS] Success! You have satisfied all execution requirements.
hacker@piping~redirecting-more-output:~$ cat myflag

[FLAG] Here is your flag:
[FLAG] pwn.college{osUY9nM7Qc-Lq2ZXYmhmmA1Udtp.dVjN1QDL5kTO0czW}
```

---

## Appending output

A common use-case of output redirection is to save off some command results for later analysis.
Often times, you want to do this in aggregate: run a bunch of commands, save their output, and grep through it later.
In this case, you might want all that output to keep appending to the same file, but `>` will create a new output file every time, deleting the old contents.

You can redirect input in append mode using `>>` instead of `>`, as so:

```
hacker@dojo:~$ echo pwn > outfile
hacker@dojo:~$ echo college >> outfile
hacker@dojo:~$ cat outfile
pwn
college
hacker@dojo:$
```

### CTF

```
hacker@piping~appending-output:~$ /challenge/run >> /home/hacker/the-flag
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge will check that output is redirected to a specific file path : /home/hacker/the-flag

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] Good luck!

[TEST] You should have redirected my stdout to a file called /home/hacker/the-flag. Checking...

[HINT] File descriptors are inherited from the parent, unless the FD_CLOEXEC is set by the parent on the file descriptor.
[HINT] For security reasons, some programs, such as python, do this by default in certain cases. Be careful if you are
[HINT] creating and trying to pass in FDs in python.

[PASS] The file at the other end of my stdout looks okay!
[PASS] Success! You have satisfied all execution requirements.
I will write the flag in two parts to the file /home/hacker/the-flag! I'll do
the first write directly to the file, and the second write, I'll do to stdout
(if it's pointing at the file). If you redirect the output in append mode, the
second write will append to (rather than overwrite) the first write, and you'll
get the whole flag!

hacker@piping~appending-output:~$ cat /home/hacker/the-flag
 |
\|/ This is the first half:
 v
pwn.college{cVfudkXUvsZfx0NoF4TllI7Z_v3.ddDM5QDL5kTO0czW}
                              ^
     that is the second half /|\
                              |

If you only see the second half above, you redirected in *truncate* mode (>)
rather than *append* mode (>>), and so the write of the second half to stdout
overwrote the initial write of the first half directly to the file. Try append
mode!
```

---

## Redirecting errors

Just like standard output, you can also redirect the error channel of commands. 
Here, we'll learn about **File Descriptor numbers**. 
***A File Descriptor (FD***) is a number the describes a communication channel in Linux. 
You've already been using them, even though you didn't realize it. We're already familiar with three:

- FD 0: Standard Input
- FD 1: Standard Output
- FD 2: Standard Error

When you redirect process communication, you do it by FD number, though some FD numbers are implicit.
For example, a `>` without a number implies `1>`, which redirects FD 1 (Standard Output).
Thus, the following two commands are equivalent:

`hacker@dojo:~$ echo hi > asdf`
`hacker@dojo:~$ echo hi 1> asdf`

Redirecting errors is pretty easy from this point.
If you have a command that might produce data via standard error (such as `/challenge/run`), you can do:

`hacker@dojo:~$ /challenge/run 2> errors.log`

That will redirect standard error (FD 2) to the `errors.log` file.
Furthermore, you can redirect multiple file descriptors at the same time! For example:

`hacker@dojo:~$ some_command > output.log 2> errors.log`

That command will redirect output to `output.log` and errors to `errors.log`.

### CTF

```
hacker@piping~redirecting-errors:~$ /challenge/run 1> myflag 2> instructions
hacker@piping~redirecting-errors:~$ cat instructions
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge will check that output is redirected to a specific file path : myflag
[INFO] - the challenge will check that error output is redirected to a specific file path : instructions
[INFO] - the challenge will output a reward file if all the tests pass : /flag

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /flag file.

[TEST] You should have redirected my stdout to a file called myflag. Checking...

[PASS] The file at the other end of my stdout looks okay!

[TEST] You should have redirected my stderr to instructions. Checking...

[PASS] The file at the other end of my stderr looks okay!
[PASS] Success! You have satisfied all execution requirements.
hacker@piping~redirecting-errors:~$ cat myflag

[FLAG] Here is your flag:
[FLAG] pwn.college{IX0LgaMPAawJflKFGQ6sx61kHu2.ddjN1QDL5kTO0czW}
```

---

## Redirecting input

Just like you can redirect output from programs, you can redirect input to programs!
This is done using `<`, as so:

```
hacker@dojo:~$ echo yo > message
hacker@dojo:~$ cat message
yo
hacker@dojo:~$ rev < message
oy
```

### CTF

```
hacker@piping~redirecting-input:~$ echo COLLEGE > PWN
hacker@piping~redirecting-input:~$ /challenge/run < PWN
Reading from standard input...
Correct! You have redirected the PWN file into my standard input, and I read
the value 'COLLEGE' out of it!
Here is your flag:
pwn.college{gN6dTayKKJ0dhQWK0s1T5qLF4Lx.dBzN1QDL5kTO0czW}
```

---

## Grepping stored results

### CTF

```
hacker@piping~grepping-stored-results:~$ /challenge/run > /tmp/data.txt
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge will check that output is redirected to a specific file path : /tmp/data.txt
[INFO] - the challenge will output a reward file if all the tests pass : /challenge/.data.txt

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /challenge/.data.txt file.

[TEST] You should have redirected my stdout to a file called /tmp/data.txt. Checking...

[HINT] File descriptors are inherited from the parent, unless the FD_CLOEXEC is set by the parent on the file descriptor.
[HINT] For security reasons, some programs, such as python, do this by default in certain cases. Be careful if you are
[HINT] creating and trying to pass in FDs in python.

[PASS] The file at the other end of my stdout looks okay!
[PASS] Success! You have satisfied all execution requirements.
hacker@piping~grepping-stored-results:~$ grep pwn.college /tmp/data.txt
pwn.college{ENny1hMOf0qlCWfr-Mjp5_7DB9M.dhTM4QDL5kTO0czW}
```

---

### Grepping live output

It turns out that you can "cut out the middleman" and avoid the need to store results to a file, like you did in the last level. 
You can use this using the `|` (pipe) operator. 
Standard output from the command to the left of the pipe will be connected to (*piped into*) the standard input of the command to the right of the pipe.
For example:

```
hacker@dojo:~$ echo no-no | grep yes
hacker@dojo:~$ echo yes-yes | grep yes
yes-yes
hacker@dojo:~$ echo yes-yes | grep no
hacker@dojo:~$ echo no-no | grep no
no-no
```

### CTF

```
hacker@piping~grepping-live-output:~$ /challenge/run | grep 'pwn.college'
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge checks for a specific process at the other end of stdout : grep
[INFO] - the challenge will output a reward file if all the tests pass : /challenge/.data.txt

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /challenge/.data.txt file.

[TEST] You should have redirected my stdout to another process. Checking...
[TEST] Performing checks on that process!

[INFO] The process' executable is /nix/store/xpq4yhadyhazkcsggmqd7rsgvxb3kjy4-gnugrep-3.11/bin/grep.
[INFO] This might be different than expected because of symbolic links (for example, from /usr/bin/python to /usr/bin/python3 to /usr/bin/python3.8).
[INFO] To pass the checks, the executable must be grep.

[PASS] You have passed the checks on the process on the other end of my stdout!
[PASS] Success! You have satisfied all execution requirements.
pwn.college{oT0aTjYfoyBaJ9SgC50plv0nFId.dlTM4QDL5kTO0czW}
```

---

## Grepping errors

The `>` operator redirects a given file descriptor to a file, and you've used `2>` to redirect fd 2, which is standard error.
The `|` operator redirects only standard output to another program, and there is no `2|` form of the operator!
It can only redirect standard output (file descriptor 1).

The shell has a `>&` operator, which redirects a file descriptor to another file descriptor.
This means that we can have a two-step process to grep through errors: 
1. first, we redirect standard error to standard output (`2>& 1`) 
2. and then pipe the now-combined stderr and stdout as normal (`|`)!

### CTF

```
hacker@piping~grepping-errors:~$ /challenge/run 2>& 1 | grep 'pwn.college'
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge checks for a specific process at the other end of stderr : grep
[INFO] - the challenge will output a reward file if all the tests pass : /challenge/.data.txt

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /challenge/.data.txt file.

[TEST] You should have redirected my stderr to another process. Checking...
[TEST] Performing checks on that process!

[INFO] The process' executable is /nix/store/xpq4yhadyhazkcsggmqd7rsgvxb3kjy4-gnugrep-3.11/bin/grep.
[INFO] This might be different than expected because of symbolic links (for example, from /usr/bin/python to /usr/bin/python3 to /usr/bin/python3.8).
[INFO] To pass the checks, the executable must be grep.

[PASS] You have passed the checks on the process on the other end of my stderr!
[PASS] Success! You have satisfied all execution requirements.
pwn.college{sfQ0Ekzk-bWt2XqsGIfmSa7ugHJ.dVDM5QDL5kTO0czW}
```

---

## Duplicating piped data with three

When you pipe data from one command to another, you of course no longer see it on your screen. 
This is not always desired: for example, you might want to see the data as it flows through between your commands to debug unintended outcomes
(e.g., "why did that second command not work???").

Luckily, there is a solution! 
The `tee` command, named after a **"T-splitter"** from plumbing pipes, duplicates data flowing through your pipes to any number of files provided on the command line.
For example:

```
hacker@dojo:~$ echo hi | tee pwn college
hi
hacker@dojo:~$ cat pwn
hi
hacker@dojo:~$ cat college
hi
hacker@dojo:~$
```

As you can see, by providing two files to `tee, we ended up with three copies of the piped-in data: 
- one to stdout, 
- one to the pwn file, 
- and one to the college file. 
You can imagine how you might use this to debug things going haywire:

```
hacker@dojo:~$ command_1 | command_2
Command 2 failed!
hacker@dojo:~$ command_1 | tee cmd1_output | command_2
Command 2 failed!
hacker@dojo:~$ cat cmd1_output
Command 1 failed: must pass --succeed!
hacker@dojo:~$ command_1 --succeed | command_2
Commands succeeded!
```

### CTF

```
hacker@piping~duplicating-piped-data-with-tee:~$ /challenge/pwn | tee pwn_output | /challenge/college
Processing...
WARNING: you are overwriting file pwn_output with tee's output...
The input to 'college' does not contain the correct secret code! This code
should be provided by the 'pwn' command. HINT: use 'tee' to intercept the
output of 'pwn' and figure out what the code needs to be.
hacker@piping~duplicating-piped-data-with-tee:~$ cat pwn_output
Usage: /challenge/pwn --secret [SECRET_ARG]

SECRET_ARG should be "YJJWC6Qh"
hacker@piping~duplicating-piped-data-with-tee:~$ /challenge/pwn --secret YJJWC6Qh | /challenge/college
Processing...
Correct! Passing secret value to /challenge/college...
Great job! Here is your flag:
pwn.college{YJJWC6QhZo_oqDCx9ZzvQo1y-58.dFjM5QDL5kTO0czW}
```

---

## Writing to multiple programs

`Tee` says in its manpage, it's designed to write to files and to standard output:

```
TEE(1)                           User Commands                          TEE(1)

NAME
       tee - read from standard input and write to standard output and files
```

Luckily, Linux follows the philosophy that "everything is a file".
This is, the system strives to provide file-like access to most resources, including the input and output of running programs!
The shell follows this philosophy, allowing you to, for example, use any utility that takes file arguments on the command line (such as tee) and hook it up to the input or output side of a program!

This is done using what's called Process Substitution.
If you write an argument of >(rev), bash will run the rev command (this command reads data from standard input, reverses its order, and writes it to standard output!), but hook up its input to a temporary file that it will create.
This isn't a real file, of course, it's what's called a named pipe, in that it has a file name:

```
hacker@dojo:~$ echo >(rev)
/dev/fd/63
hacker@dojo:~$
```

Where did `/dev/fd/63` come from? `bash` replaced `>(rev)` with the path of the named pipe file that's hooked up to `rev`'s input!
While the command is running, writing to this file will pipe data to the standard input of the command.
Typically, this is done using commands that take output files as arguments (like `tee`):

```
hacker@dojo:~$ echo HACK | rev
KCAH
hacker@dojo:~$ echo HACK | tee >(rev)
HACK
KCAH
```

Above, the following sequence of events took place:

1. `bash` started up the `rev` command, hooking a named pipe (presumably `/dev/fd/63`) to `rev`'s standard input
2. `bash` started up the `tee` command, hooking a pipe to its standard input, and replacing the first argument to `tee` with `/dev/fd/63`.
    tee never even saw the argument `>(rev)`; the shell substituted it before launching `tee`
3. `bash` used the `echo` builtin to print `HACK` into `tee`'s standard input
4. `tee` read `HACK`, wrote it to standard output, and then wrote it to `/dev/fd/63` (which is connected to `rev`'s stdin)
5. `rev` read `HACK` from its standard input, reversed it, and wrote `KCAH` to standard output

> Trivia!

> The observant learner will realize that the following are equivalant:

```
hacker@dojo:~$ echo hi | rev
ih
hacker@dojo:~$ echo hi > >(rev)
ih
hacker@dojo:~$
```

> More than one way to pipe data! Of course, the second route is way harder to read and also harder to expand. For example:

```
hacker@dojo:~$ echo hi | rev | rev
hi
hacker@dojo:~$ echo hi > >(rev | rev)
hi
hacker@dojo:~$
```

> That's just silly! The lesson here is that, while Process Substitution is a powerful tool in your toolbox, it's a very specialized tool; don't use it for everything!

### CTF

```
hacker@piping~writing-to-multiple-programs:~$ /challenge/hack | tee >(/challenge/the) > >(/challenge/planet)
Congratulations, you have duplicated data into the input of two programs! Here
is your flag:
pwn.college{kEU7bWqKkzIsZk7Lk1t0jVEvyb7.dBDO0UDL5kTO0czW}
```

---

## Split-piping stderr and stdout

### CTF

```
hacker@piping~split-piping-stderr-and-stdout:~$ /challenge/hack 2> >(/challenge/the) > >(/challenge/planet)
Congratulations, you have learned a redirection technique that even experts
struggle with! Here is your flag:
pwn.college{gOVlFkwvltKbrsri-6ab3FFeZsP.dFDNwYDL5kTO0czW}
```

---
