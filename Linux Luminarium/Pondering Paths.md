# Pondering Paths

The Linux filesystem is a "tree". That is, it has a root (written as ***/***).
The root of the filesystem is a directory, and every directory can contain other directories and files.
You refer to files and directories by their path.
A path from the root of the filesystem starts with ***/*** (that is, the root of the filesystem), and describes the set of directories that must be descended into to find the file.
Every piece of the path is demarcated with another ***/***.

---

## The root

You can invoke a program by providing its path on the command line. 
In this case, you'll be giving the exact path, starting from ***/***.
This style of path, one that starts with the root directory, is referred to as an ***absolute path***.

### CTF

```
hacker@paths~the-root:~$ /pwn
BOOM!!!
Here is your flag:
pwn.college{83vDBNrpi3O_0lcXEfNQQZo5k3F.dhzN5QDL5kTO0czW}
```

---

## Program and absolute paths

### CTF

```
hacker@paths~program-and-absolute-paths:~$ /challenge/run
Correct!!!
/challenge/run is an absolute path! Here is your flag:
pwn.college{8hUf2BszjYdAbeQS6WyS_FGTStE.dVDN1QDL5kTO0czW}
```

---

## Position thy self

The Linux filesystem has tons of directories with tons of files.
You can navigate around directories by using the ***cd*** (**c**hange **d**irectory) command and passing a path to it as an argument, as so:

```
hacker@dojo:~$ cd /some/new/directory
hacker@dojo:~/some/new/directory$ cd /some/new/directory
```

This affects the "current working directory" of your process (in this case, the bash shell).
Each process has a directory in which it's currently hanging out.

### CTF

```
hacker@paths~position-elsewhere:~$ /challenge/run
Incorrect...
You are not currently in the /sys directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~position-thy-self:~$ cd /sys
hacker@paths~position-thy-self:/sys$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
pwn.college{kbCl98XqrWmC6lUaMFm38UA_vxG.dZDN1QDL5kTO0czW}
```

---

## Position elsewhere

### CTF

```
hacker@paths~position-elsewhere:~$ /challenge/run
Incorrect...
You are not currently in the /var/lib/apt/lists directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~position-elsewhere:~$ cd /var/lib/apt/lists
hacker@paths~position-elsewhere:/var/lib/apt/lists$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
pwn.college{MAJ2u285SM-HAEsEsilnOL_8wm_.ddDN1QDL5kTO0czW}
```

---

## Position yet elsewhere

### CTF

```
hacker@paths~position-yet-elsewhere:~$ /challenge/run
Incorrect...
You are not currently in the /proc/67 directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~position-yet-elsewhere:~$ cd /proc/67
hacker@paths~position-yet-elsewhere:/proc/67$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
pwn.college{Q-8X8d5vtOTvR1NDshxpOxo3NP-.dhDN1QDL5kTO0czW}
```

---

## implicit relative paths, from /

If you put in absolute paths everywhere, then it really doesn't matter what directory you are in.
However, the current working directory does matter for relative paths.

- A relative path is any path that does not start at root (i.e., it does not start with ***/***).
- A relative path is interpreted **relative** to your **c**urrent **w**orking **d**irectory (***cwd***).
- Your ***cwd*** is the directory that your prompt is currently located at.

This means how you specify a particular file, depends on where the terminal prompt is located.

Imagine we want to access some file located at **/tmp/a/b/my_file**.

- If my **cwd** is ***/***, then a relative path to the file is ***tmp/a/b/my_file***.
- If my **cwd** is ***/tmp***, then a relative path to the file is ***a/b/my_file***.
- If my **cwd** is ***/tmp/a/b/c***, then a relative path to the file is ***../my_file***. The ***..*** refers to the parent directory.

Note : The `pwd` command stands for 'print working directory'. It displays the full, absolute path of the current directory you are in. This can be very useful when you're working with relative paths, as it helps you understand your current location in the filesystem hierarchy.

### CTF

```
hacker@paths~implicit-relative-paths-from-:/$ pwd
/
hacker@paths~implicit-relative-paths-from-:/$ challenge/run
Correct!!!
challenge/run is a relative path, invoked from the right directory!
Here is your flag:
pwn.college{ABxZ1xlzyLn76soOSQ8H1j2Kqm6.dlDN1QDL5kTO0czW}
```

---


## explicit relative paths, from /

In most operating systems, including Linux, every directory has two implicit entries that you can reference in paths: **.** and **..**. The first, **.**, refers right to the same directory, so the following absolute paths are all identical to each other:

- **/challenge**
- **/challenge/.**
- **/challenge/./././././././././**
- **/./././challenge/././**

The following relative paths are also all identical to each other:

- **challenge**
- **./challenge**
- **./././challenge**
- **challenge/.**

Of course, if your current working directory is **/**, the above relative paths are equivalent to the above absolute paths.

> ChatGPT

>The entries . (single dot) and .. (double dots) are special references that are available in every directory of most operating systems, including Linux. These entries make navigating file systems more convenient.
Understanding the . (Single Dot)

>The . represents the current directory. It's a shorthand that refers to the directory you're currently in, meaning it points to "right here".
Example:

>Let's say you're in a directory called /challenge. Any reference to . will refer to the /challenge directory itself.

>So, these paths:

    /challenge
    /challenge/.
    /challenge/./././././././

>are all exactly the same because:

    /challenge: This is a direct path to the /challenge directory.
    /challenge/.: Adding a single . doesn’t change the location—you're still referring to the /challenge directory.
    /challenge/./././: The repeated . just means "stay in the current directory" multiple times, which still leaves you in /challenge.

>Understanding Relative Paths with .

>Relative paths are paths that don't start with a / (the root directory). They reference locations relative to the current working directory.

>For example, if your current directory is / (the root directory), and you want to navigate to /challenge:

    challenge: This is a relative path that assumes you're starting in the / directory.
    ./challenge: Here, the . represents the current directory, so ./challenge is saying "from the current directory, go to the challenge directory," which is exactly the same as challenge.
    challenge/.: This is saying "from the challenge directory, stay in the challenge directory."

>All of these relative paths are functionally identical and will lead you to the same location.
Example of Absolute vs. Relative Paths:

>If your current directory is /:

    Absolute path: /challenge will take you directly to /challenge.
    Relative path: ./challenge will take you to /challenge because . refers to the current directory, which is /.

>Summary of Key Points:

    . (Single Dot): Refers to the current directory. It can be used in both absolute and relative paths, but it doesn't change where you're pointing—it just confirms you're in the current directory.
    Absolute Paths: Start from / (root), and paths like /challenge/. or /challenge/././. all mean the same thing as /challenge because the . keeps you in the current directory.
    Relative Paths: Start from your current working directory. The relative path ./challenge is equivalent to challenge and will lead you to the same place.

>Essentially, the . in these paths doesn't change the destination—it just reaffirms that you're pointing to the current directory at various points in your path.

### CTF

```
hacker@paths~explicit-relative-paths-from-:~$ pwd
/home/hacker
hacker@paths~explicit-relative-paths-from-:~$ cd /./
hacker@paths~explicit-relative-paths-from-:/$ pwd
/
hacker@paths~explicit-relative-paths-from-:/$ ./challenge/run
Correct!!!
./challenge/run is a relative path, invoked from the right directory!
Here is your flag:
pwn.college{o8s3ohZi-E17s3Ro17UZOlrWB0O.dBTN1QDL5kTO0czW}
```

---

## implicit relative path

Linux explicitly avoids automatically looking in the current directory when you provide a "naked" path. Consider the following:

```
hacker@dojo:~$ cd /challenge
hacker@dojo:/challenge$ run
```

This will not invoke **/challenge/run**. This is actually a safety measure: if Linux searched the current directory for programs every time you entered a naked path, you could accidentally execute programs in your current directory that happened to have the same names as core system utilities! As a result, the above commands will yield the following error:

`bash: run: command not found`

### CTF

```
hacker@paths~implicit-relative-path:~$ pwd
/home/hacker
hacker@paths~implicit-relative-path:~$ cd /./
hacker@paths~implicit-relative-path:/$ pwd
/
hacker@paths~implicit-relative-path:/$ cd ./challenge
hacker@paths~implicit-relative-path:/challenge$ ./run
Correct!!!
./run is a relative path, invoked from the right directory!
Here is your flag:
pwn.college{MmLLDU-LHGwp3M4hbsWEkRaRTpZ.dFTN1QDL5kTO0czW}
```

---


## home sweet home

Every user has a home directory, typically under **/home** in the filesystem. In the dojo, you are the **hacker** user, and your home directory is **/home/hacker**. The home directory is typically where users store most of their personal files. As you make your way through pwn.college, this is where you'll store most of your solutions.

Typically, your shell session will start with your home directory as your current working directory. Consider the initial prompt:

`hacker@dojo:~$`

The **~** in this prompt is the current working directory, with **~** being shorthand for **/home/hacker**. Bash provides and uses this shorthand because, again, most of your time will be spent in your home directory. Thus, whenever bash sees **~** provided as the start of an argument in a way consistent with a path, it will expand it to your home directory. Consider:

```
hacker@dojo:~$ echo LOOK: ~
LOOK: /home/hacker
hacker@dojo:~$ cd /
hacker@dojo:/$ cd ~
hacker@dojo:~$ cd ~/asdf
hacker@dojo:~/asdf$ cd ~/asdf
hacker@dojo:~/asdf$ cd ~
hacker@dojo:~$ cd /home/hacker/asdf
hacker@dojo:~/asdf$
```

Note that the expansion of **~** is an absolute path, and only the leading **~** is expanded. This means, for example, that **~/~** will be expanded to **/home/hacker/~** rather than **/home/hacker/home/hacker**.

Fun fact: **cd** will use your home directory as the default destination:

```
hacker@dojo:~$ cd /tmp
hacker@dojo:/tmp$ cd
hacker@dojo:~$
```

Note : The `cat` command in Linux is short for "concatenate" and is commonly used to read and display the contents of files. When you use `cat` followed by a filename, it outputs the file's contents to the terminal.

For example, if you have a file named `example.txt`, you can view its contents by typing:

`cat example.txt`

### CTF

```
hacker@paths~home-sweet-home:~$ pwd
/home/hacker
hacker@paths~home-sweet-home:~$ /challenge/run ~/a
Writing the file to /home/hacker/a!
... and reading it back to you:
pwn.college{gzeJ91Gn9CDlphq1AbQVWm3FNfQ.dNzM4QDL5kTO0czW}
```

---




