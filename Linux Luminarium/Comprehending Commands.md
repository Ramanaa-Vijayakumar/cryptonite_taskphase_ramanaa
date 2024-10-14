# Comprehending Commands

---

## cat: not the pet, but the command!

One of the most critical Linux commands is `cat`. 
`cat` is most often used for reading out files, like so:

`hacker@dojo:~$ cat /challenge/DESCRIPTION.md`

One of the most critical Linux commands is `cat`.
`cat` is most often used for reading out files, like so:

`cat` will con**cat**enate (hence the name) multiple files if provided multiple arguments.
For example:

```
hacker@dojo:~$ cat myfile
This is my file!
hacker@dojo:~$ cat yourfile
This is your file!
hacker@dojo:~$ cat myfile yourfile
This is my file!
This is your file!
hacker@dojo:~$ cat myfile yourfile myfile
This is my file!
This is your file!
This is my file!
```

Finally, if you give no arguments at all, `cat` will read from the terminal input and output it.

### CTF

```
hacker@commands~cat-not-the-pet-but-the-command:~$ ls
Desktop  a  flag  leap
hacker@commands~cat-not-the-pet-but-the-command:~$ cat flag
pwn.college{YcihLsggAch87-RbxSf3u8H-y9N.dFzN1QDL5kTO0czW}
```

---

## catting absolute paths

Note : You can, of course, specify `cat`'s arguments as absolute paths:

`hacker@dojo:~$ cat /challenge/DESCRIPTION.md`

In the last level, you did `cat flag` to read the flag out of your home directory!
You can, of course, specify `cat`'s arguments as absolute paths:
...

### CTF

```
hacker@commands~catting-absolute-paths:~$ pwd
/home/hacker
hacker@commands~catting-absolute-paths:~$ cd ..
hacker@commands~catting-absolute-paths:/home$ cd ..
hacker@commands~catting-absolute-paths:/$ ls
bin   challenge  etc   home  lib32  libx32  mnt  opt   root  sbin  sys  usr
boot  dev        flag  lib   lib64  media   nix  proc  run   srv   tmp  var
hacker@commands~catting-absolute-paths:/$ cat /flag
pwn.college{U788LEU88RSXnoHMs7NWVxvrdJ3.dlTM5QDL5kTO0czW}
```

---

## more catting practice

Note : You can specify all sorts of paths as arguments to commands

### CTF

```
hacker@commands~more-catting-practice:~$ cat /usr/include/c++/flag
pwn.college{A7oIAAnTtG7E5AjhS8R0WTQpTJT.dBjM5QDL5kTO0czW}
```

---

## grepping for a needle in a haystack

Sometimes, the files that you might `cat` out are too big. 
Luckily, we have the `grep` command to search for the contents we need!

There are many ways to `grep`, and we'll learn on way here:

`hacker@dojo:~$ grep SEARCH_STRING /path/to/file`

Invoked like this, ***grep*** will search the file for lines of text containing ***SEARCH_STRING*** and print them to the console.

### CTF

```
hacker@commands~grepping-for-a-needle-in-a-haystack:~$ grep pwn.college /challenge/data.txt
pwn.college{I3p-DOhWPmKqWS29HAAYbnEpUIh.ddTM4QDL5kTO0czW}
```

---

## listing files

`ls` will **l**i**s**t files in all the directories provided to it as arguments,
and in the current directory if no arguments are provided. Observe:

```
hacker@dojo:~$ ls /challenge
run
hacker@dojo:~$ ls
Desktop    Downloads  Pictures  Templates
Documents  Music      Public    Videos
hacker@dojo:~$ ls /home/hacker
Desktop    Downloads  Pictures  Templates
Documents  Music      Public    Videos
hacker@dojo:~$
```

### CTF

```
hacker@commands~listing-files:/$ ls /challenge
28301-renamed-run-6386  DESCRIPTION.md
hacker@commands~listing-files:/$ cd /challenge
acker@commands~listing-files:/challenge$ cat ./28301-renamed-run-6386
#!/opt/pwn.college/bash

echo "Yahaha, you found me! Here is your flag:"
cat /flag
hacker@commands~listing-files:/challenge$ ./28301-renamed-run-6386
Yahaha, you found me! Here is your flag:
pwn.college{Yu8J3p7Hh_cirZj5EBNDqDViws3.dhjM4QDL5kTO0czW}
```

---

## touching files

You can create a new, blank file by touching it with the `touch` command:

```
hacker@dojo:~$ cd /tmp
hacker@dojo:/tmp$ ls
hacker@dojo:/tmp$ touch pwnfile
hacker@dojo:/tmp$ ls
pwnfile
hacker@dojo:/tmp$
```

### CTF

```
hacker@commands~touching-files:~$ touch /tmp/pwn
hacker@commands~touching-files:~$ touch /tmp/college
hacker@commands~touching-files:~$ cd /tmp
hacker@commands~touching-files:/tmp$ ls
bin  college  hsperfdata_root  pwn  tmp.XvrUsDZh8M
hacker@commands~touching-files:/tmp$ /challenge/run
Success! Here is your flag:
pwn.college{QIiAviq2ARxtFgmwOdNKTQuLkH6.dBzM4QDL5kTO0czW}
```

---

## removing files

In Linux, you **r**e**m**ove files with the `rm` command, as so:

```
hacker@dojo:~$ touch PWN
hacker@dojo:~$ touch COLLEGE
hacker@dojo:~$ ls
COLLEGE     PWN
hacker@dojo:~$ rm PWN
hacker@dojo:~$ ls
COLLEGE
hacker@dojo:~$
```

### CTF

```
hacker@commands~removing-files:~$ ls
Desktop  a  delete_me  leap
hacker@commands~removing-files:~$ rm delete_me
hacker@commands~removing-files:~$ ls
Desktop  a  leap
hacker@commands~removing-files:~$ /challenge/check
Excellent removal. Here is your reward:
pwn.college{Qk2m4bqYrIsCdMGBTdAnRZluIrt.dZTOwUDL5kTO0czW}
```

---

## hidden files

Interestingly, `ls` doesn't list all the files by default. Linux has a convention where files that start with a `.` don't show up by default in `ls` and in a few other contexts. To view them with `ls`, you need to invoke `ls` with the `-a` flag, as so:

```
hacker@dojo:~$ touch pwn
hacker@dojo:~$ touch .college
hacker@dojo:~$ ls
pwn
hacker@dojo:~$ ls -a
.college	pwn
hacker@dojo:~$
```

### CTF

```
hacker@commands~hidden-files:~$ pwd
/home/hacker
hacker@commands~hidden-files:~$ ls
Desktop  a  leap
hacker@commands~hidden-files:~$ ls -a
.   .ICEauthority  .bash_logout  .cache   .dbus   .profile  a
..  .bash_history  .bashrc       .config  .local  Desktop   leap
hacker@commands~hidden-files:~$ ls -a /
.           .flag-206252583911894  challenge  home   lib64   mnt  proc  sbin  tmp
..          bin                    dev        lib    libx32  nix  root  srv   usr
.dockerenv  boot                   etc        lib32  media   opt  run   sys   var
hacker@commands~hidden-files:~$ /challenge/run
There's no flag here for you! I made the flag readable but hid it as a .-prepended file in the / directory. Go find it!
hacker@commands~hidden-files:~$ cat /.flag-206252583911894
pwn.college{M1PUIAzCP9W-iUBj0twYrE-CwoF.dBTN4QDL5kTO0czW}
```

---

## An Epic Filesystem Quest

> SENSAI

> Note: `grep` is a powerful tool for searching text within files, but it might not be the best fit when we want to read the file's content directly.

> Note: `cat` is used to read the contents of a specific file, not a directory.

> Note: `cat` to read the contents of a file without needing to change directories.

### CTF

```
hacker@commands~an-epic-filesystem-quest:~$ pwd
/home/hacker
hacker@commands~an-epic-filesystem-quest:~$ ls
Desktop  a  leap
hacker@commands~an-epic-filesystem-quest:~$ cd /
hacker@commands~an-epic-filesystem-quest:/$ ls
MEMO  boot       dev  flag  lib    lib64   media  nix  proc  run   srv  tmp  var
bin   challenge  etc  home  lib32  libx32  mnt    opt  root  sbin  sys  usr
hacker@commands~an-epic-filesystem-quest:/$ cat MEMO
Congratulations, you found the clue!
The next clue is in: /opt/linux/linux-5.4/drivers/staging/comedi/drivers/tests

Watch out! The next clue is **trapped**. You'll need to read it out without 'cd'ing into the directory; otherwise, the clue will self destruct!
hacker@commands~an-epic-filesystem-quest:/$ cat MEMO
Congratulations, you found the clue!
The next clue is in: /opt/linux/linux-5.4/drivers/staging/comedi/drivers/tests

Watch out! The next clue is **trapped**. You'll need to read it out without 'cd'ing into the directory; otherwise, the clue will self destruct!
hacker@commands~an-epic-filesystem-quest:/$ cat /opt/linux/linux-5.4/drivers/staging/comedi/drivers/tests
cat: /opt/linux/linux-5.4/drivers/staging/comedi/drivers/tests: Is a directory
hacker@commands~an-epic-filesystem-quest:/$ ls /opt/linux/linux-5.4/drivers/staging/comedi/drivers/tests
Makefile  TRACE-TRAPPED  example_test.c  ni_routes_test.c  unittest.h
hacker@commands~an-epic-filesystem-quest:/$ cat TRACE-TRAPPED
cat: TRACE-TRAPPED: No such file or directory
hacker@commands~an-epic-filesystem-quest:/$ ls TRACE-TRAPPED
ls: cannot access 'TRACE-TRAPPED': No such file or directory
hacker@commands~an-epic-filesystem-quest:/$ cd TRACE-TRAPPED
ssh-entrypoint: cd: TRACE-TRAPPED: No such file or directory
hacker@commands~an-epic-filesystem-quest:/$ cat /opt/linux/linux-5.4/drivers/staging/comedi/drivers/tests/TRACE-TRAPPED
Congratulations, you found the clue!
The next clue is in: /usr/share/icons/hicolor/36x36/actions

Watch out! The next clue is **trapped**. You'll need to read it out without 'cd'ing into the directory; otherwise, the clue will self destruct!
hacker@commands~an-epic-filesystem-quest:/$ ls /usr/share/icons/hicolor/36x36/actions
CUE-TRAPPED
hacker@commands~an-epic-filesystem-quest:/$ cat /usr/share/icons/hicolor/36x36/actions/CUE-TRAPPED
Congratulations, you found the clue!
The next clue is in: /opt/pwndbg/.venv/lib/python3.8/site-packages/cryptography/hazmat/primitives/serialization/__pycache__
hacker@commands~an-epic-filesystem-quest:/$ ls /opt/pwndbg/.venv/lib/python3.8/site-packages/cryptography/hazmat/primitives/serialization/__pycache__
MESSAGE                  base.cpython-38.pyc    pkcs7.cpython-38.pyc
__init__.cpython-38.pyc  pkcs12.cpython-38.pyc  ssh.cpython-38.pyc
hacker@commands~an-epic-filesystem-quest:/$ cat /opt/pwndbg/.venv/lib/python3.8/site-packages/cryptography/hazmat/primitives/serialization/__pycache__/MESSAGE
Great sleuthing!
The next clue is in: /usr/lib/python3/dist-packages/sage/knots/__pycache__

Watch out! The next clue is **trapped**. You'll need to read it out without 'cd'ing into the directory; otherwise, the clue will self destruct!
hacker@commands~an-epic-filesystem-quest:/$ ls ^C
hacker@commands~an-epic-filesystem-quest:/$ ls /usr/lib/python3/dist-packages/sage/knots/__pycache__
GIST-TRAPPED  __init__.cpython-38.pyc  all.cpython-38.pyc  gauss_code.cpython-38.pyc  knot.cpython-38.pyc  knot_table.cpython-38.pyc  link.cpython-38.pyc
hacker@commands~an-epic-filesystem-quest:/$ cat ^C
hacker@commands~an-epic-filesystem-quest:/$ /opt/pwndbg/.venv/lib/python3.8/site-^C
hacker@commands~an-epic-filesystem-quest:/$ cat /opt/pwndbg/.venv/lib/python3.8/site-packages/cryptography/hazmat/primitives/serialization/__pycache__/GIST-TRAPPED
cat: /opt/pwndbg/.venv/lib/python3.8/site-packages/cryptography/hazmat/primitives/serialization/__pycache__/GIST-TRAPPED: No such file or directory
hacker@commands~an-epic-filesystem-quest:/$ cat /usr/lib/python3/dist-packages/sage/knots/__pycache__/GIST-TRAPPED
Lucky listing!
The next clue is in: /opt/linux/linux-5.4/drivers/gpu/drm/amd/display/dc/dml

The next clue is **hidden** --- its filename starts with a '.' character. You'll need to look for it using special options to 'ls'.
hacker@commands~an-epic-filesystem-quest:/$ ls -a /opt/linux/linux-5.4/drivers/gpu/drm/amd/display/dc/dml
.      Makefile       dcn21                 display_mode_lib.h      display_mode_vba.h        dml1_display_rq_dlg_calc.c  dml_common_defs.h
..     dc_features.h  display_mode_enums.h  display_mode_structs.h  display_rq_dlg_helpers.c  dml1_display_rq_dlg_calc.h  dml_inline_defs.h
.HINT  dcn20          display_mode_lib.c    display_mode_vba.c      display_rq_dlg_helpers.h  dml_common_defs.c           dml_logger.h
hacker@commands~an-epic-filesystem-quest:/$ cat /opt/linux/linux-5.4/drivers/gpu/drm/amd/display/dc/dml/.HINT
Lucky listing!
The next clue is in: /opt/linux/linux-5.4/include/linux/soc/sunxi

Watch out! The next clue is **trapped**. You'll need to read it out without 'cd'ing into the directory; otherwise, the clue will self destruct!
hacker@commands~an-epic-filesystem-quest:/$ ls /opt/linux/linux-5.4/include/linux/soc/sunxi
BLUEPRINT-TRAPPED  sunxi_sram.h
hacker@commands~an-epic-filesystem-quest:/$ cat /opt/linux/linux-5.4/include/linux/soc/sunxi/BLUEPRINT-TRAPPED
Great sleuthing!
The next clue is in: /usr/share/doc/gir1.2-pango-1.0

The next clue is **delayed** --- it will not become readable until you enter the directory with 'cd'.
hacker@commands~an-epic-filesystem-quest:/$ cd /usr/share/doc/gir1.2-pango-1.0
hacker@commands~an-epic-filesystem-quest:/usr/share/doc/gir1.2-pango-1.0$ ls
BRIEF  changelog.Debian.gz  copyright
hacker@commands~an-epic-filesystem-quest:/usr/share/doc/gir1.2-pango-1.0$ cat BRIEF
Yahaha, you found me!
The next clue is in: /opt/linux/linux-5.4/tools/arch/s390/include/uapi/asm

The next clue is **hidden** --- its filename starts with a '.' character. You'll need to look for it using special options to 'ls'.
hacker@commands~an-epic-filesystem-quest:/usr/share/doc/gir1.2-pango-1.0$ ls -a /opt/linux/linux-5.4/tools/arch/s390/include/uapi/asm
.  ..  .README  bitsperlong.h  bpf_perf_event.h  kvm.h  kvm_perf.h  mman.h  perf_regs.h  ptrace.h  sie.h
hacker@commands~an-epic-filesystem-quest:/usr/share/doc/gir1.2-pango-1.0$ cat /opt/linux/linux-5.4/tools/arch/s390/include/uapi/asm/.README
CONGRATULATIONS! Your perserverence has paid off, and you have found the flag!
It is: pwn.college{gQqs8Khv1j6-_nzMJVtP1XkN4Yz.dljM4QDL5kTO0czW}
```

---

## making directories

You **m**a**k**e **dir**ectories using the mkdir command.

```
hacker@dojo:~$ cd /tmp
hacker@dojo:/tmp$ ls
hacker@dojo:/tmp$ ls
hacker@dojo:/tmp$ mkdir my_directory
hacker@dojo:/tmp$ ls
my_directory
hacker@dojo:/tmp$ cd my_directory
hacker@dojo:/tmp/my_directory$ touch my_file
hacker@dojo:/tmp/my_directory$ ls
my_file
hacker@dojo:/tmp/my_directory$ ls /tmp/my_directory/my_file
/tmp/my_directory/my_file
hacker@dojo:/tmp/my_directory$
```

### CTF

```
hacker@commands~making-directories:~$ pwd
/home/hacker
hacker@commands~making-directories:~$ mkdir -p /tmp/pwn
hacker@commands~making-directories:~$ ls
Desktop  a  leap
hacker@commands~making-directories:~$ cd /tmp/pwn
hacker@commands~making-directories:/tmp/pwn$ touch college
hacker@commands~making-directories:/tmp/pwn$ ls
college
hacker@commands~making-directories:/tmp/pwn$ /challenge/run
Success! Here is your flag:
pwn.college{4xtwQMQHHLNl9lWNH2xCPUGFVV-.dFzM4QDL5kTO0czW}
```

---

## finding files

The `find` command takes optional arguments describing the search criteria and the search location. If you don't specify a search criteria, `find` matches every file. If you don't specify a search location, `find` uses the current working directory (.). 
For example:

```
hacker@dojo:~$ mkdir my_directory
hacker@dojo:~$ mkdir my_directory/my_subdirectory
hacker@dojo:~$ touch my_directory/my_file
hacker@dojo:~$ touch my_directory/my_subdirectory/my_subfile
hacker@dojo:~$ find
.
./my_directory
./my_directory/my_subdirectory
./my_directory/my_subdirectory/my_subfile
./my_directory/my_file
hacker@dojo:~$
```

And when specifying the search location:

```
hacker@dojo:~$ find my_directory/my_subdirectory
my_directory/my_subdirectory
my_directory/my_subdirectory/my_subfile
hacker@dojo:~$
```

And, of course, we can specify the criteria! For example, here, we filter by name:

```
hacker@dojo:~$ find -name my_subfile
./my_directory/my_subdirectory/my_subfile
hacker@dojo:~$ find -name my_subdirectory
./my_directory/my_subdirectory
hacker@dojo:~$
```

You can search the whole filesystem if you want!

```
hacker@dojo:~$ find / -name hacker
/home/hacker
hacker@dojo:~$
```

### CTF

```
hacker@commands~finding-files:~$ find / -name flag
find: ‘/root’: Permission denied
find: ‘/proc/1/task/1/fd’: Permission denied
find: ‘/proc/1/task/1/fdinfo’: Permission denied
find: ‘/proc/1/task/1/ns’: Permission denied
find: ‘/proc/1/fd’: Permission denied
find: ‘/proc/1/map_files’: Permission denied
find: ‘/proc/1/fdinfo’: Permission denied
find: ‘/proc/1/ns’: Permission denied
find: ‘/proc/7/task/7/fd’: Permission denied
find: ‘/proc/7/task/7/fdinfo’: Permission denied
find: ‘/proc/7/task/7/ns’: Permission denied
find: ‘/proc/7/fd’: Permission denied
find: ‘/proc/7/map_files’: Permission denied
find: ‘/proc/7/fdinfo’: Permission denied
find: ‘/proc/7/ns’: Permission denied
find: ‘/var/log/private’: Permission denied
find: ‘/var/log/apache2’: Permission denied
find: ‘/var/log/mysql’: Permission denied
find: ‘/var/cache/ldconfig’: Permission denied
find: ‘/var/cache/apt/archives/partial’: Permission denied
find: ‘/var/cache/private’: Permission denied
find: ‘/var/lib/apt/lists/partial’: Permission denied
find: ‘/var/lib/php/sessions’: Permission denied
find: ‘/var/lib/mysql-files’: Permission denied
find: ‘/var/lib/private’: Permission denied
find: ‘/var/lib/mysql-keyring’: Permission denied
find: ‘/var/lib/mysql’: Permission denied
find: ‘/tmp/tmp.XvrUsDZh8M’: Permission denied
find: ‘/run/mysqld’: Permission denied
find: ‘/run/sudo’: Permission denied
find: ‘/etc/ssl/private’: Permission denied
/usr/local/lib/python3.8/dist-packages/pwnlib/flag
/usr/local/share/radare2/5.9.5/flag
/usr/share/singular/factory/gftables/flag
/opt/pwndbg/.venv/lib/python3.8/site-packages/pwnlib/flag
/opt/radare2/libr/flag
/nix/store/pmvk2bk4p550w182rjfm529kfqddnvh3-python3.11-pwntools-4.12.0/lib/python3.11/site-packages/pwnlib/flag
/nix/store/1yagn5s8sf7kcs2hkccgf8d0wxlrv5sz-radare2-5.9.0/share/radare2/5.9.0/flag
hacker@commands~finding-files:~$ cat ^C
hacker@commands~finding-files:~$ cat /usr/local/lib/python3.8/dist-packages/pwnlib/flag
cat: /usr/local/lib/python3.8/dist-packages/pwnlib/flag: Is a directory
hacker@commands~finding-files:~$ cat /usr/local/share/radare2/5.9.5/flag
cat: /usr/local/share/radare2/5.9.5/flag: Is a directory
hacker@commands~finding-files:~$ cat /usr/share/singular/factory/gftables/flag
pwn.college{gDzjGKYrSRs8q7jaGPjF-dsdX-N.dJzM4QDL5kTO0czW}
```

---

## linking files

If you use Linux (or computers) for any reasonable length of time to do any real work, you will eventually run into some variant of the following situation: you want two programs to access the same data, but the programs expect that data to be in two different locations. Luckily, Linux provides a solution to this quandry: **links**.

Links come in two flavors: hard and soft (also known as symbolic) links. We'll differentiate the two with an analogy:

- A hard link is when you address your appartment using multiple addresses that all lead directly to the same place (e.g., *Apt 2* vs *Unit 2*).
- A soft link is when you move appartments and have the postal service automatically forward your mail from your old place to your new place.

In a filesystem, a file is, conceptually, an address at which the contents of that file live. A hard link is an alternate address that indexes that data --- accesses to the hard link and accesses to the original file are completely identical, in that they immediate yield the necessary data. A soft/symbolic link, instead, contains the original file name. When you access the symbolic link, Linux will realize that it is a symbolic link, read the original file name, and then (typically) automatically access that file. In most cases, both situations result in accessing the original data, but the mechanisms are different.

**Symbolic links** (also also known as ***symlinks***) are created with the `ln` command with the `-s` argument, like so:

```
hacker@dojo:~$ cat /tmp/myfile
This is my file!
hacker@dojo:~$ ln -s /tmp/myfile /home/hacker/ourfile
hacker@dojo:~$ cat ~/ourfile
This is my file!
hacker@dojo:~$
```

Note: the original file path comes before the link path in the command!

A symlink can be identified as such with a few methods. For example, the `file` command, which takes a filename and tells you what type of file it is, will recognize symlinks:

```
hacker@dojo:~$ file /tmp/myfile
/tmp/myfile: ASCII text
hacker@dojo:~$ file ~/ourfile
/home/hacker/ourfile: symbolic link to /tmp/myfile
hacker@dojo:~$
```

### CTF

```
hacker@commands~linking-files:~$ ln -s /flag /home/hacker/not-the-flag
hacker@commands~linking-files:~$ /challenge/catflag
About to read out the /home/hacker/not-the-flag file!
pwn.college{0xu8umoXKALL_18zZZpXSBmX-Qp.dlTM1UDL5kTO0czW}
```

---