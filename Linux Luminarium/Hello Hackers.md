# Hello Hackers

The command line lets you execute commands. 
When you launch a terminal, it will execute a command line **shell** which will look like this:

`hacker@dojo:~$`

This is called the **prompt**, and it's prompting you to enter a command.

- The ***hacker*** in the prompt is the username of the current user.
  In the pwn.college DOJO enviromment, this is ***hacker***.
- The ***dojo*** part of the prompt is the hostname of the machine the shell is on
  (this remainder can be useful you are a system administrator who deals with many machines on a daily basis, for example).
  In the example above, the hostname is ***dojo***, but in pwn.college, it will be derived from the name of the challenge you're attempting.
- ***~*** shows the current that your shell is located at.
- The ***$*** at the end of the prompt signifies that ***hacker*** is not an administrative user.
  In much later modules in pwn.college, when you learn to use exploits to become the administrative user,
  you will see the prompt signify that by printing ***#*** instead of ***$***, and you'll know that you've won!

---

[extensive bash tutorial](https://bash.cyberciti.biz/guide/Main_Page)

---

## Intro to Commands  

When you type a command and hit enter, the command will be invoked, as so:

```
hacker@dojo:~$ whoami
hacker
hacker@dojo:~$
```

Here, the user executed the ***whoami*** command, which simply prints the username (***hacker***) to the terminal.
When the command terminates, the shell once again displays the prompt, ready for the next command.
Keep in mind: commands in Linux are case sensitive: **hello** is different from **HELLO**.

### CTF

```
hacker@hello~intro-to-commands:~$ hello
Success! Here is your flag:
pwn.college{AnUbq4wgesaoqgDu0VdzxGG31tY.ddjNyUDL5kTO0czW}
```

---

## Intro to Arguments

When you type a line of text and hit enter, the shell actually parses your input into a command and its arguments.
The first word is the command, and the subsequent words are arguments. Observe:

```
hacker@dojo:~$ echo Hello
Hello
hacker@dojo:~$
```

In this case, the command was ***echo***, and the argument was ***Hello***.
***echo*** is a simple command that "echoes" all of its arguments back out onto the terminal.

Let's look at echo with multiple arguments:

```
hacker@dojo:~$ echo Hello Hackers!
Hello Hackers!
hacker@dojo:~$
```

In this case, the command was ***echo***, and ***Hello*** and ***Hackers!*** were the two arguments to ***echo***. Simple!

### CTF

```
hacker@hello~intro-to-arguments:~$ hello hackers
Success! Here is your flag:
pwn.college{0qvXG2ysi7zvUQvGHNwXjCYMnA3.dhjNyUDL5kTO0czW}
```

---








