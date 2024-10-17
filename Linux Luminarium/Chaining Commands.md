# Chaining Commands

## Chaining with Semicolons

The easiest way to chain commands is `;`. In most contexts, `;` separates commands in a similar way to how Enter separates lines.
So, this:

```
hacker@dojo:~$ echo COLLEGE > pwn
hacker@dojo:~$ cat pwn
COLLEGE
hacker@dojo:~$
```

Is roughly the same as this:

```
hacker@dojo:~$ echo COLLEGE > pwn; cat pwn
COLLEGE
hacker@dojo:~$
```

Basically, when you hit Enter, your shell executes your typed command and, after that command terminates, give you the prompt to input another command. The semicolon is analogous, just without the prompt and with you entering both commands before anything is executed.

### CTF

```
hacker@chaining~chaining-with-semicolons:~$ /challenge/pwn; /challenge/college
Yes! You chained /challenge/pwn and /challenge/college! Here is your flag:
pwn.college{swfg4U1t46Rxg_MQLDTcRnLKGN5.dVTN4QDL5kTO0czW}
```

---

## Your First Shell Script

As you combine more and more commands to achieve complex effects, the length of the combined prompt quickly gets really annoying to deal with. When this happens, you can put these commands in a file, called a shell script, and run them by executing the file! For example, consider our semicolon technique:

```
hacker@dojo:~$ echo COLLEGE > pwn; cat pwn
COLLEGE
hacker@dojo:~$
```

We can create a shell script called `pwn.sh` (by convention, shell scripts are frequently named with a `sh` suffix):

```
echo COLLEGE > pwn
cat pwn
```

And then we can execute by passing it as an argument to a new instance of our shell (`bash`)! When a shell is invoked like this, rather than taking commands from the user, it reads commands from the file.

```
hacker@dojo:~$ ls
hacker@dojo:~$ bash pwn.sh
COLLEGE
hacker@dojo:~$ ls
pwn
hacker@dojo:~$
```

You can see that the shell script executed both commands, creating and printing the `pwn` file.

> NOTE: We haven't yet talked about Linux's amazing array of competent command line file editors. For now, feel free to use the `Text Editor` application in Desktop mode (`Applications->Accessories->Text Editor`) or the default editor in the VSCode Workspace!

### CTF

```
hacker@chaining~your-first-shell-script:~$ nano x.sh
hacker@chaining~your-first-shell-script:~$ bash x.sh
Great job, you've written your first shell script! Here is the flag:
pwn.college{gApsZB1nWmtJeJ_WB8FNv0ty3gj.dFzN4QDL5kTO0czW}
```

---

## Redirecting Script Output

You've piped output between programs with `|`, but so far, this has just been between one command's output and a different command's input. But what if you wanted to send the output of several programs to one command? There are a few ways to do this, and we'll explore a simple one here: redirecting output from your script!

As far as the shell is concerned, your script is just another command. That means you can redirect its input and output just like you did for commands in the `Piping` module! For example, you can write it to a file:

```
hacker@dojo:~$ cat script.sh
echo PWN
echo COLLEGE
hacker@dojo:~$ bash script.sh > output
hacker@dojo:~$ cat output
PWN
COLLEGE
hacker@dojo:~$
```

All of the various redirection methods work: `>` for stdout, `2>` for stderr, `<` for stdin, `>>` and `2>>` for append-mode redirection, `>&` for redirecting to other file descriptors, and `|` for piping to another command.

### CTF

```
hacker@chaining~redirecting-script-output:~$ nano x.sh
hacker@chaining~redirecting-script-output:~$ bash x.sh | /challenge/solve
Correct! Here is your flag:
pwn.college{g6wO5orLYCgQmK9QMmUVxSpKLyl.dhTM5QDL5kTO0czW}
```

---

## Executable Shell Scripts

You have written your first shell script, but calling it via `bash script.sh` is a pain. Why do you need that `bash`?

When you invoke `bash script.sh`, you are, of course launching the `bash` command with the `script.sh` argument. This tells bash to read its commands from `script.sh` instead of standard input, and thus your shell script is executed.

It turns out that you can avoid the need to manually invoke `bash`. If your shell script file is ***executable*** (recall [File Permissions](https://pwn.college/linux-luminarium/permissions)), you can simply invoke it via its relative or absolute path! For example, if you create `script.sh` in your home directory and make it executable, you can invoke it via `/home/hacker/script.sh` or `~/script`.sh or (if your working directory is `/home/hacker`) `./script.sh`.

### CTF

```
hacker@chaining~executable-shell-scripts:~$ nano x.sh
hacker@chaining~executable-shell-scripts:~$ pwd
/home/hacker
hacker@chaining~executable-shell-scripts:~$ ./x.sh
ssh-entrypoint: ./x.sh: Permission denied
hacker@chaining~executable-shell-scripts:~$ ls -l x.sh
-rw-r--r-- 1 hacker hacker 17 Oct 17 17:48 x.sh
hacker@chaining~executable-shell-scripts:~$ chmod a=rwx x.sh
hacker@chaining~executable-shell-scripts:~$ ./x.sh
Congratulations on your shell script execution! Your flag:
pwn.college{EdT3z5B0dmw8OyoiVCOyYFvgywZ.dRzNyUDL5kTO0czW}
```

---