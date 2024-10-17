# Untangling Users

You have experience with two users: `hacker` and `root`. These are just two users, and there are MANY more on a typical Linux system! The full list of users on a Linux system is specified in the `/etc/passwd` file (named so for historical reasons --- it doesn't actually hold passwords anymore). Here is an example from the dojo container:

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
systemd-timesync:x:101:101:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
systemd-network:x:102:103:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:103:104:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
mysql:x:104:105:MySQL Server,,,:/nonexistent:/bin/false
messagebus:x:105:106::/nonexistent:/usr/sbin/nologin
sshd:x:106:65534::/run/sshd:/usr/sbin/nologin
hacker:x:1000:1000::/home/hacker:/bin/bash
```

A lot of users here, and a lot of info! Each line contains, separated by `:`s, the username, an `x` as a placeholder for where the password used to be (we'll cover where it actually is later), the numerical user ID, the numerical default group ID, long-form user details, the home directory, and the default shell.

We can see `hacker` and `root`, along with a bunch of others. Many are there for historical reasons, some are service accounts to support various installed software, and some are "utility" accounts (e.g., the `nobody` user is used to ensure that, e.g., a program runs without any special privileges).

---

## Becoming root with su

In the previous level, you used the `/challenge/getroot` program to become the `root` user. Becoming root is a fairly common action that Linux users take, and your typical Linux installation obviously does not have `/challenge/getroot`. Instead, there are two utilities used for this purposes: `su` and `sudo`.

In this challenge, we will cover the older one, `su` (the **s**witch **u**ser command). This is not typically used to elevate to root access anymore, but it is an elegant utility from a more civilized time, and we'll cover it first.

`su` is a setuid binary:

```
hacker@dojo:~$ ls -l /usr/bin/su
-rwsr-xr-x 1 root root 232416 Dec 1 11:45 /usr/bin/su
hacker@dojo:~$
```

Because it has the SUID bit set, `su` runs as root. Running as root, it can start a root shell! Of course, `su` is discerning: before allowing the user to elevate privileges to root, it checks to make sure that the user knows the root password:

```
hacker@dojo:~$ su
Password: 
su: Authentication failure
hacker@dojo:~$
```

This check against the root password is what obsoletes `su`. Modern systems very rarely have root passwords, and different mechanisms (that we will learn later) are used to grant administrative access.

### CTF

```
hacker@users~becoming-root-with-su:~$ su
Password:
root@users~becoming-root-with-su:/home/hacker# cat /flag
pwn.college{k9wX2ZidtO6j_5_K40MhcUhbFZD.dVTN0UDL5kTO0czW}
```

---

## Other users with su

With no arguments, `su` will start a root shell (after authenticating with root's password). However, you can also give a username as an argument to switch to that user instead of root. For example:

```
hacker@dojo:~$ su some-user
Password:
some-user@dojo:~$
```

### CTF

```
hacker@users~other-users-with-su:~$ su zardus
Password:
zardus@users~other-users-with-su:/home/hacker$ /challenge/run
Congratulations, you have become Zardus! Here is your flag:
pwn.college{cf9rcLD_Nu0lhdjutTTxOZhhdzM.dZTN0UDL5kTO0czW}
```

---

## Cracking passwords

When you enter a password for `su`, it compares it against the stored password for that user. These passwords used to be stored in `/etc/passwd`, but because `/etc/passwd` is a globally-readable file, this is not good for passwords, these were moved to `/etc/shadow`. Here is the example `/etc/shadow` from the previous level:

```
root:$6$s74oZg/4.RnUvwo2$hRmCHZ9rxX56BbjnXcxa0MdOsW2moiW8qcAl/Aoc7NEuXl2DmJXPi3gLp7hmyloQvRhjXJ.wjqJ7PprVKLDtg/:19921:0:99999:7:::
daemon:*:19873:0:99999:7:::
bin:*:19873:0:99999:7:::
sys:*:19873:0:99999:7:::
sync:*:19873:0:99999:7:::
games:*:19873:0:99999:7:::
man:*:19873:0:99999:7:::
lp:*:19873:0:99999:7:::
mail:*:19873:0:99999:7:::
news:*:19873:0:99999:7:::
uucp:*:19873:0:99999:7:::
proxy:*:19873:0:99999:7:::
www-data:*:19873:0:99999:7:::
backup:*:19873:0:99999:7:::
list:*:19873:0:99999:7:::
irc:*:19873:0:99999:7:::
gnats:*:19873:0:99999:7:::
nobody:*:19873:0:99999:7:::
_apt:*:19873:0:99999:7:::
systemd-timesync:*:19901:0:99999:7:::
systemd-network:*:19901:0:99999:7:::
systemd-resolve:*:19901:0:99999:7:::
mysql:!:19901:0:99999:7:::
messagebus:*:19901:0:99999:7:::
sshd:*:19901:0:99999:7:::
hacker::19916:0:99999:7:::
zardus:$6$bEFkpM0w/6J0n979$47ksu/JE5QK6hSeB7mmuvJyY05wVypMhMMnEPTIddNUb5R9KXgNTYRTm75VOu1oRLGLbAql3ylkVa5ExuPov1.:19921:0:99999:7:::
```

Separated by :s, the first field of each line is the username and the second is the password. A value of `*` or `!` functionally means that password login for the account is disabled, a blank field means that there is no password (a not-uncommon misconfiguration that allows password-less `su` in some configurations), and the long string such as Zardus' ```$6$bEFkpM0w/6J0n979$47ksu/JE5QK6hSeB7mmuvJyY05wVypMhMMnEPTIddNUb5R9KXgNTYRTm75VOu1oRLGLbAql3ylkVa5ExuPov1```. is the result of one-way-encrypting (hashing) Zardus' password from the last level (in this case, `dont-hack-me`). Other fields in this file have other meanings, and you can read more about them [here](https://www.cyberciti.biz/faq/understanding-etcshadow-file/).

When you input a password into `su`, it one-way-encrypts (hashes) it and compares the result against the stored value. If the result matches, `su` grants you access to the user!

But what if you don't know the password? If you have the hashed value of the password, you can crack it! Even though `/etc/shadow` is, by default, only readable by root, leaks can happen! For example, backups are often stored, unencrypted and insufficiently protected, on file servers, and this has led to countless data disclosures.

If a hacker gets their hands on a leaked `/etc/shadow`, they can start cracking passwords and wreaking havoc. The cracking can be done via the famous `John the Ripper`, as so:

```
hacker@dojo:~$ john ./my-leaked-shadow-file
Loaded 1 password hash (crypt, generic crypt(3) [?/64])
Will run 32 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
password1337      (zardus)
1g 0:00:00:22 3/3 0.04528g/s 10509p/s 10509c/s 10509C/s lykys..lank
Use the "--show" option to display all of the cracked passwords reliably
Session completed
hacker@dojo:~$
```

Here, John the Ripper cracked Zardus' leaked password hash to find the real value of `password1337`. Poor Zardus!

### CTF

```
hacker@users~cracking-passwords:~$ cat /challenge/shadow-leak
root:*:19873:0:99999:7:::
daemon:*:19873:0:99999:7:::
bin:*:19873:0:99999:7:::
sys:*:19873:0:99999:7:::
sync:*:19873:0:99999:7:::
games:*:19873:0:99999:7:::
man:*:19873:0:99999:7:::
lp:*:19873:0:99999:7:::
mail:*:19873:0:99999:7:::
news:*:19873:0:99999:7:::
uucp:*:19873:0:99999:7:::
proxy:*:19873:0:99999:7:::
www-data:*:19873:0:99999:7:::
backup:*:19873:0:99999:7:::
list:*:19873:0:99999:7:::
irc:*:19873:0:99999:7:::
gnats:*:19873:0:99999:7:::
nobody:*:19873:0:99999:7:::
_apt:*:19873:0:99999:7:::
hacker::20000:0:99999:7:::
zardus:$6$I4qF.BP933gHHCzJ$Vu5Bapjc7lwWfsSAg35oB6rswT8LIGQAkmA.kE17GrI9FBA3QdouAZu.rPt.kanWUA6d8gYCPSpb8CJJqg/h./:20013:0:99999:7:::
hacker@users~cracking-passwords:~$ john /challenge/shadow-leak
Loaded 1 password hash (crypt, generic crypt(3) [?/64])
Press 'q' or Ctrl-C to abort, almost any other key for status
0g 0:00:00:14 0% 2/3 0g/s 287.5p/s 287.5c/s 287.5C/s Alexis..bigred
aardvark         (zardus)
1g 0:00:00:20 100% 2/3 0.04897g/s 285.1p/s 285.1c/s 285.1C/s Johnson..buzz
Use the "--show" option to display all of the cracked passwords reliably
Session completed
hacker@users~cracking-passwords:~$ su zardus
Password:
zardus@users~cracking-passwords:/home/hacker$ /challenge/run
Congratulations, you have become Zardus! Here is your flag:
pwn.college{EO4ewdbbg-1KAxryqbTRTZcN96c.ddTN0UDL5kTO0czW}
```

---

## Using sudo

In the olden days, a typical Linux system had a root password that administrators would use to `su` to root (after logging into their account with their normal account password). But root passwords are a pain to maintain, they (or their hashes!) can leak, and they don't lend themselves well to larger environments (e.g., fleets of servers). To address this, in recent decades, the world has moved from administration via `su` to administration via `sudo` (superuser do).

Unlike `su`, which defaults to launching a shell as a specified user, `sudo` defaults to running a command as root:

```
hacker@dojo:~$ whoami
hacker
hacker@dojo:~$ sudo whoami
root
hacker@dojo:~$
```

Or, more relevant to getting flags:

```
hacker@dojo:~$ grep hacker /etc/shadow
grep: /etc/shadow: Permission denied
hacker@dojo:~$ sudo grep hacker /etc/shadow
hacker:$6$Xro.e7qB3Q2Jl2sA$j6xffIgWn9xIxWUeFzvwPf.nOH2NTWNJCU5XVkPuONjIC7jL467SR4bXjpVJx4b/bkbl7kyhNquWtkNlulFoy.:19921:0:99999:7:::
hacker@dojo:~$
```

Also unlike `su`, rather than authenticating via password, `sudo` relies on policies that it checks to determine the user's authorization run things as root. These policies are defined in `/etc/sudoers`, and though it's mostly out of scale for our purposes, there are plenty of [resources](https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file) for learning about this!

So, the world has moved to `sudo` and has (for the purposes of system administration) left `su` behind. In fact, even pwn.college's Practice Mode works by giving you `sudo` access to elevate privileges!

### CTF

```
hacker@users~using-sudo:~$ sudo cat /flag
pwn.college{sC3LqFbYmg-3JSdS7GJW84HJV5k.dhTN0UDL5kTO0czW}
```

---