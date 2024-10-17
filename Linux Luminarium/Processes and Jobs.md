# Processes and Jobs 

In modern computing, this software is split into two categories: 

1. operating system kernels
2. processes

When Linux starts up, it launches an init (short for initializer) process that, in turn, launches a bunch of other processes which launch more processes until, eventually, you are looking at your command line shell, which is also a process! The shell, of course, launches processes in response to the commands you enter.

---

## Listing Processes

`ps` either stands for **"process snapshot"** or **"process status"**, and it lists processes. By default, `ps` just lists the processes running in your terminal, which honestly isn't very useful:

```
hacker@dojo:~$ ps
    PID TTY          TIME CMD
    329 pts/0    00:00:00 bash
    349 pts/0    00:00:00 ps
hacker@dojo:~$
```

In the above example, we have the shell (`bash`) and the `ps` process itself, and that's all that's running on that specific terminal. We also see that each process has a numerical identifier (the Process ID, or PID), which is a number that uniquely identifies every running process in a Linux environment. We also see the terminal on which the commands are running (in this case, the designation `pts/0`), and the total amount of cpu time that the process has eaten up so far (since these processes are very undemanding, they have yet to eat up even 1 second!).

As `ps` is a very old utility, its usage is a bit of a mess. There are two ways to specify arguments.

"Standard" Syntax: in this syntax, you can use `-e` to list "every" process and `-f` for a "full format" output, including arguments. These can be combined into a single argument `-ef`.

"BSD" Syntax: in this syntax, you can use `a` to list processes for all users, `x` to list processes that aren't running in a terminal, and `u` for a "user-readable" output. These can be combined into a single argument `aux`.

These two methods, `ps -ef` and `ps aux` result in slightly different, but cross-recognizable output.

Let's try it in the dojo:

```
hacker@dojo:~$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
hacker         1       0  0 05:34 ?        00:00:00 /sbin/docker-init -- /bin/sleep 6h
hacker         7       1  0 05:34 ?        00:00:00 /bin/sleep 6h
hacker       102       1  1 05:34 ?        00:00:00 /usr/lib/code-server/lib/node /usr/lib/code-server --auth=none -
hacker       138     102 11 05:34 ?        00:00:07 /usr/lib/code-server/lib/node /usr/lib/code-server/out/node/entr
hacker       287     138  0 05:34 ?        00:00:00 /usr/lib/code-server/lib/node /usr/lib/code-server/lib/vscode/ou
hacker       318     138  6 05:34 ?        00:00:03 /usr/lib/code-server/lib/node --dns-result-order=ipv4first /usr/
hacker       554     138  3 05:35 ?        00:00:00 /usr/lib/code-server/lib/node /usr/lib/code-server/lib/vscode/ou
hacker       571     554  0 05:35 pts/0    00:00:00 /usr/bin/bash --init-file /usr/lib/code-server/lib/vscode/out/vs
hacker       695     571  0 05:35 pts/0    00:00:00 ps -ef
hacker@dojo:~$
```

You can see here that there are processes running for the initialization of the challenge environment (`docker-init`), a timeout before the challenge is automatically terminated to preserve computing resources (`sleep 6h` to timeout after 6 hours), the VSCode environment (several `code-server` helper processes), the shell (`bash`), and my `ps -ef` command.
It's basically the same thing with `ps aux`:

```
hacker@dojo:~$ ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
hacker         1  0.0  0.0   1128     4 ?        Ss   05:34   0:00 /sbin/docker-init -- /bin/sleep 6h
hacker         7  0.0  0.0   2736   580 ?        S    05:34   0:00 /bin/sleep 6h
hacker       102  0.4  0.0 723944 64660 ?        Sl   05:34   0:00 /usr/lib/code-server/lib/node /usr/lib/code-serve
hacker       138  3.3  0.0 968792 106272 ?       Sl   05:34   0:07 /usr/lib/code-server/lib/node /usr/lib/code-serve
hacker       287  0.0  0.0 717648 53136 ?        Sl   05:34   0:00 /usr/lib/code-server/lib/node /usr/lib/code-serve
hacker       318  3.3  0.0 977472 98256 ?        Sl   05:34   0:06 /usr/lib/code-server/lib/node --dns-result-order=
hacker       554  0.4  0.0 650560 55360 ?        Rl   05:35   0:00 /usr/lib/code-server/lib/node /usr/lib/code-serve
hacker       571  0.0  0.0   4600  4032 pts/0    Ss   05:35   0:00 /usr/bin/bash --init-file /usr/lib/code-server/li
hacker      1172  0.0  0.0   5892  2924 pts/0    R+   05:38   0:00 ps aux
hacker@dojo:~$
```

There are many commonalities between `ps -ef` and `ps aux`: both display the user (`USER` column), the PID, the TTY, the start time of the process (`STIME/START`), the total utilized CPU time (`TIME`), and the command (`CMD/COMMAND`).
`ps -ef` additionally outputs the Parent Process ID (`PPID`), which is the PID of the process that launched the one in question, while `ps aux` outputs the percentage of total system CPU and Memory that the process is utilizing.

>NOTE: Both `ps -ef` and `ps aux` truncate the command listing to the width of your terminal (which is why the examples above line up so nicely on the right side of the screen. If you can't read the whole path to the process, you might need to enlarge your terminal (or redirect the output somewhere to avoid this truncating behavior)!

### CTF

```
hacker@processes~listing-processes:~$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 12:24 ?        00:00:00 /sbin/docker-init -- /nix/var/ni
root           7       1  0 12:24 ?        00:00:00 /run/dojo/bin/sleep 6h
root          68       1  0 12:24 ?        00:00:00 /challenge/2729-run-23964
root          72      68  0 12:24 ?        00:00:00 sleep 6h
hacker        73       0  0 12:24 pts/0    00:00:00 /run/dojo/bin/ssh-entrypoint
hacker        92      73  0 12:26 pts/0    00:00:00 ps -ef
hacker@processes~listing-processes:~$ ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0   1056   640 ?        Ss   12:24   0:00 /sbin/docker-init
root           7  0.0  0.0   5052  2240 ?        S    12:24   0:00 /run/dojo/bin/sle
root          68  0.0  0.0   4132  2560 ?        S    12:24   0:00 /challenge/2729-r
root          72  0.0  0.0   2744  1280 ?        S    12:24   0:00 sleep 6h
hacker        73  0.0  0.0   5372  3840 pts/0    Ss   12:24   0:00 /run/dojo/bin/ssh
hacker        93  0.0  0.0   7868  3200 pts/0    R+   12:27   0:00 ps aux
hacker@processes~listing-processes:~$ /challenge/2729-run-23964
Yahaha, you found me! Here is your flag:
pwn.college{Y28m_SjvuApdShzdDy49Y0VywLD.dhzM4QDL5kTO0czW}
```

---

## Killing Processes

You've launched processes, you've viewed processes, now you will learn to terminate processes! In Linux, this is done using the aggressively-named `kill` command.
`kill` will terminate a process in a way that gives it a chance to get its affairs in order before ceasing to exist.

Let's say you had a pesky `sleep` process (`sleep` is a program that simply hangs out for the number of seconds specified on the commandline, in this case, 1337 seconds) that you launched in another terminal, like so:

```
hacker@dojo:~$ sleep 1337
```

How do we get rid of it? You use `kill` to terminate it by passing the process identifier (the `PID` from `ps`) as an argument, like so:

```
hacker@dojo:~$ ps -e | grep sleep
 342 pts/0    00:00:00 sleep
hacker@dojo:~$ kill 342
hacker@dojo:~$ ps -e | grep sleep
hacker@dojo:~$
```

### CTF

```
hacker@processes~killing-processes:~$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 12:34 ?        00:00:00 /sbin/docker-init -- /nix/var/ni
root           7       1  0 12:34 ?        00:00:00 /run/dojo/bin/sleep 6h
root          71       1  0 12:34 ?        00:00:00 su -c /challenge/.launcher hacke
hacker        73      71  0 12:34 ?        00:00:00 /challenge/dont_run
hacker        74      73  0 12:34 ?        00:00:00 sleep 6h
hacker        75       0  0 12:34 pts/0    00:00:00 /run/dojo/bin/ssh-entrypoint
hacker        92      75  0 12:34 pts/0    00:00:00 ps -ef
hacker@processes~killing-processes:~$ kill 73
hacker@processes~killing-processes:~$ /challenge/run
Great job! Here is your payment:
pwn.college{MeRzjGHNnin0ub9NasUKHdQ5Ryj.dJDN4QDL5kTO0czW}
```

---

## Interrupting Processes

but sometimes you just want to get rid of the process that's clogging up your terminal instead of killing other processes with the `kill` command! Luckily, terminals have a hotkey for this: `Ctrl-C` (e.g., holding down the `Ctrl` key and pressing `C`) sends an "interrupt" to whatever application is waiting on input from the terminal and, typically, this causes the application to cleanly exit.
[terminals and "control codes"](https://catern.com/posts/terminal_quirks.html)

### CTF

```
hacker@processes~interrupting-processes:~$ /challenge/run
I could give you the flag... but I won't, until this process exits. Remember,
you can force me to exit with Ctrl-C. Try it now!
^C
Good job! You have used Ctrl-C to interrupt this process! Here is your flag:
pwn.college{QnLc3yZrKKUswXyC6tTcDUdGlEl.dNDN4QDL5kTO0czW}
```

---

## Suspending Processes

We can interrupt processes with `Ctrl-C`, but there are less drastic measures you can use to get your terminal back! You can suspend processes to the background with `Ctrl-Z`.

### CTF

```
hacker@processes~suspending-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running in
this terminal... Let's check!

UID          PID    PPID  C STIME TTY          TIME CMD
root          85      65  0 12:41 pts/0    00:00:00 bash /challenge/run
root          87      85  0 12:41 pts/0    00:00:00 ps -f

I don't see a second me!

To pass this level, you need to suspend me and launch me again! You can
background me with Ctrl-Z or, if you're not ready to do that for whatever
reason, just hit Enter and I'll exit!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~suspending-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running in
this terminal... Let's check!

UID          PID    PPID  C STIME TTY          TIME CMD
root          85      65  0 12:41 pts/0    00:00:00 bash /challenge/run
root          92      65  0 12:41 pts/0    00:00:00 bash /challenge/run
root          94      92  0 12:41 pts/0    00:00:00 ps -f

Yay, I found another version of me! Here is the flag:
pwn.college{AXZapOoXn93dMKtrNNJ-PZkxyOv.dVDN4QDL5kTO0czW}
```

---

## Resuming Processes

Usually, when you suspend processes, you'll want to resume them at some point. Otherwise, why not just terminate them? To resume processes, your shell provides the `fg` command, a builtin that takes the suspended process, resumes it, and puts it back in the foreground of your terminal.

### CTF

```
hacker@processes~resuming-processes:~$ /challenge/run
Let's practice resuming processes! Suspend me with Ctrl-Z, then resume me with
the 'fg' command! Or just press Enter to quit me!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~resuming-processes:~$ fg
/challenge/run
I'm back! Here's your flag:
pwn.college{YPO9mdfLqqqNBqU6N-0npkZ5bLL.dZDN4QDL5kTO0czW}
```

---

## Backgrounding Processes

You've resumed processes in the ***foreground*** with the `fg` command. You can also resume processes in the ***background*** with the `bg` command! This will allow the process to keep running, while giving you your shell back to invoke more commands in the meantime.

> ARCANUM: If you're interested in some deeper details, check out how to view the differences between suspended and backgrounded properties! Allow me to demonstrate. First, let's suspend a `sleep`:

```
hacker@dojo:~$ sleep 1337
^Z
[1]+  Stopped                 sleep 1337
hacker@dojo:~$
```

> The `sleep` process is now suspended in the background. We can see this with `ps` by enabling the `stat` column output with the `-o` option:

```
hacker@dojo:~$ ps -o user,pid,stat,cmd
USER         PID STAT CMD
hacker       702 Ss   bash
hacker       762 T    sleep 1337
hacker       782 R+   ps -o user,pid,stat,cmd
hacker@dojo:~$ 
```

> See that `T`? That means that the process is suspended due to our `Ctrl-Z`.
The `S` in `bash`'s `STAT` column means that `bash` is sleeping while waiting for input.
the `R` in `ps`'s column means that it's actively running, and the `+` means that it's in the foreground!

> Watch what happens when we resume `sleep` in the background:

```
hacker@dojo:~$ bg
[1]+ sleep 1337 &
hacker@dojo:~$ ps -o user,pid,stat,cmd
USER         PID STAT CMD
hacker       702 Ss   bash
hacker       762 S    sleep 1337
hacker      1224 R+   ps -o user,pid,stat,cmd
hacker@dojo:~$
```

> Boom! The `sleep` now has an `S`.
It's sleeping while, well, sleeping, but it's not suspended!
It's also in the background and thus doesn't have the `+`.

### CTF

```
hacker@processes~backgrounding-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running *and
not suspended* in this terminal... Let's check!

UID          PID STAT CMD
root          83 S+   bash /challenge/run
root          85 R+   ps -o user=UID,pid,stat,cmd

I don't see a second me!

To pass this level, you need to suspend me, resume the suspended process in the
background, and then launch a new version of me! You can background me with
Ctrl-Z (and resume me in the background with 'bg') or, if you're not ready to
do that for whatever reason, just hit Enter and I'll exit!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~backgrounding-processes:~$ bg
[1]+ /challenge/run &



Yay, I'm now running the background! Because of that, this text will probably
overlap weirdly with the shell prompt. Don't panic; just hit Enter a few times
to scroll this text out.
hacker@processes~backgrounding-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running *and
not suspended* in this terminal... Let's check!

UID          PID STAT CMD
root          83 S    bash /challenge/run
root          93 S    sleep 6h
root          94 S+   bash /challenge/run
root          96 R+   ps -o user=UID,pid,stat,cmd

Yay, I found another version of me running in the background! Here is the flag:
pwn.college{sZ8IneHIDFsTlJPmAGFtTq_l_bx.ddDN4QDL5kTO0czW}
```

## Foregrounding Processes

Imagine that you have a backgrounded process, and you want to mess with it some more. What do you do? Well, you can foreground a backgrounded process with `fg` just like you foreground a suspended process!

### CTF

```
hacker@processes~foregrounding-processes:~$ /challenge/run
To pass this level, you need to suspend me, resume the suspended process in the
background, and *then* foreground it without re-suspending it! You can
background me with Ctrl-Z (and resume me in the background with 'bg') or, if
you're not ready to do that for whatever reason, just hit Enter and I'll exit!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~foregrounding-processes:~$ bg
[1]+ /challenge/run &



Yay, I'm now running the background! Because of that, this text will probably
overlap weirdly with the shell prompt. Don't panic; just hit Enter a few times
to scroll this text out. After that, resume me into the foreground with 'fg';
I'll wait.
hacker@processes~foregrounding-processes:~$ fg
/challenge/run
YES! Great job! I'm now running in the foreground. Hit Enter for your flag!

pwn.college{4fvHSRmC7_KSqxMvmkUWASCmbtJ.dhDN4QDL5kTO0czW}
```

## Starting Background Processes

Of course, you don't have to suspend processes to background them: you can start the backgrounded right off the bat! It's easy; all you have to do is append a `&` to the command, like so:

```
hacker@dojo:~$ sleep 1337 &
[1] 1771
hacker@dojo:~$ ps -o user,pid,stat,cmd
USER         PID STAT CMD
hacker      1709 Ss   bash
hacker      1771 S    sleep 1337
hacker      1782 R+   ps -o user,pid,stat,cmd
hacker@dojo:~$
```

Here, `sleep` is actively running in the background, not suspended.

### CTF

```
hacker@processes~starting-backgrounded-processes:~$ /challenge/run &
[1] 82



hacker@processes~starting-backgrounded-processes:~$ Yay, you started me in the background! Because of that, this text will probably
overlap weirdly with the shell prompt, but you're used to that by now...

Anyways! Here is your flag!
pwn.college{0arVHFCuJ1N0PXHOj4wx514PWUP.dlDN4QDL5kTO0czW}
```

---

## Process Exit codes

Every shell command, including every program and every builtin, exits with an exit code when it finishes running and terminates, This can be used by the shell, or the user of the shell (that's you!) to check if the process succeeded in its functionality (this determination, of course, depends on what the process is supposed to do in the first place).

You can access the exit code of the most recently-terminated command using the special `?` variable (don't forget to prepend it with `$` to read its value!):

```
hacker@dojo:~$ touch test-file
hacker@dojo:~$ echo $?
0
hacker@dojo:~$ touch /test-file
touch: cannot touch '/test-file': Permission denied
hacker@dojo:~$ echo $?
1
hacker@dojo:~$
```

As you can see, commands that succeed typically return `0` and commands that fail typically return a non-zero value, most commonly `1` but sometimes an error code that identifies a specific failure mode.

### CTF

```
hacker@processes~process-exit-codes:~$ /challenge/get-code
Exiting with an error code!
hacker@processes~process-exit-codes:~$ echo $?
6
hacker@processes~process-exit-codes:~$ /challenge/submit-code 6
CORRECT! Here is your flag:
pwn.college{Uby_xo9nLkNrqaiSnLainMkkW7J.dljN4UDL5kTO0czW}
```

---
