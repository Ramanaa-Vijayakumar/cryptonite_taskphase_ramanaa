# Shell Variables

Fun fact : The Linux command line interface is colloquially referred to as a "shell", and hence programs written in this language are referred to as "shell scripts".

[Shell Variables](https://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-5.html)

---

## Printing Variables

You can accomplish this using a number of ways, but we'll start with `echo`. This command just prints stuff. For example:

```
hacker@dojo:~$ echo Hello Hackers!
Hello Hackers!
```

You can also print out variables with `echo`, by prepending the variable name with a `$`. For example, there is a variable, `PWD`, that always holds the current working directory of the current shell. You print it out as so:

```
hacker@dojo:~$ echo $PWD
/home/hacker
```

### CTF

```
hacker@variables~printing-variables:~$ echo $FLAG
pwn.college{8eCaFid3xHuvasPU2ieSxzEursW.ddTN1QDL5kTO0czW}
```

## Setting Variables

Naturally, as well as reading values stored in variables, you can write values to variables. This is done, as with many other languages, using `=`. To set variable `VAR` to value `1337`, you would use:

`hacker@dojo:~$ VAR=1337`

Note that there are no spaces around the =! If you put spaces (e.g., `VAR = 1337`), the shell won't recognize a variable assignment and will, instead, try to run the `VAR` command (which does not exist).

Also note that this uses `VAR` and not `$VAR`: the `$` is only prepended to access variables. In shell terms, this prepending of `$` triggers what is called variable expansion, and is, surprisingly, the source of many potential vulnerabilities (if you're interested in that, check out the Art of the Shell dojo when you get comfortable with the command line!).

After setting variables, you can access them using the techniques you've learned previously, such as:

```
hacker@dojo:~$ echo $VAR
1337
```

### CTF

```
hacker@variables~setting-variables:~$ PWN=COLLEGE
You've set the PWN variable properly! As promised, here is the flag:
pwn.college{g53S8SVp8rue0bqT-z7pEVqRc6C.dlTN1QDL5kTO0czW}
```

---

## Multi-word Variables

In this level, you will learn about quoting. Spaces have special significance in the shell, and there are places where you can't use them spuriously. Recall our variable setting:

`hacker@dojo:~$ VAR=1337`

That sets the `VAR` variable to `1337`, but what if you wanted to set it to `1337 SAUCE`? You might try the following:

`hacker@dojo:~$ VAR=1337 SAUCE`

This looks reasonable, but it does not work, for similar reasons to needing to have no spaces around the `=`. When the shell sees a space, it ends the variable assignment and interprets the next word (`SAUCE` in this case) as a command. To set `VAR` to `1337 SAUCE`, you need to quote it:

`hacker@dojo:~$ VAR="1337 SAUCE"`

### CTF

```
hacker@variables~multi-word-variables:~$ PWN="COLLEGE YEAH"
You've set the PWN variable properly! As promised, here is the flag:
pwn.college{w14aRJLGga7Qgj4KQzCOmLMGUyF.dBjN1QDL5kTO0czW}
```

---

## Exporting Variables

By default, variables that you set in a shell session are local to that shell process. That is, other commands you run won't inherit them. You can experiment with this by simply invoking another shell process in your own shell, like so:

```
hacker@dojo:~$ VAR=1337
hacker@dojo:~$ echo "VAR is: $VAR"
VAR is: 1337
hacker@dojo:~$ sh
$ echo "VAR is: $VAR"
VAR is: 
```

In the output above, the `$` prompt is the prompt of `sh`, a minimal shell implementation that invoked as a child of the main shell process. And it does not receive the `VAR` variable!

This makes sense, of course. Your shell variables might have sensitive or weird data, and you don't want it leaking to other programs you run unless it explicitly should. How do you mark that it should? You export your variables. When you export your variables, they are passed into the environment variables of child processes. You'll encounter the concept of environment variables in other challenges, but you'll observe their effects here. Here is an example:

```
hacker@dojo:~$ VAR=1337
hacker@dojo:~$ export VAR
hacker@dojo:~$ sh
$ echo "VAR is: $VAR"
VAR is: 1337
```

Here, the child shell received the value of VAR and was able to print it out! You can also combine those first two lines.

```
hacker@dojo:~$ export VAR=1337
hacker@dojo:~$ sh
$ echo "VAR is: $VAR"
VAR is: 1337
```

### CTF

```
hacker@variables~exporting-variables:~$ PWN=COLLEGE
You've set the PWN variable to the proper value!
hacker@variables~exporting-variables:~$ export PWN
You've set the PWN variable to the proper value!
hacker@variables~exporting-variables:~$ COLLEGE=PWN
You've set the PWN variable to the proper value!
You've set the COLLEGE variable to the proper value!
hacker@variables~exporting-variables:~$ sh
sh-5.2$ /challenge/run
CORRECT!
You have exported PWN=COLLEGE and set, but not exported, COLLEGE=PWN. Great
job! Here is your flag:
pwn.college{MxAAdNk6pn8Ch4WOlSJnpiYMGgQ.dJjN1QDL5kTO0czW}
```

---

## Printing Exported Variables

Try the `env` command: it'll print out every exported variable set in your shell.

### CTF

```
hacker@variables~printing-exported-variables:~$ env
SHELL=/run/dojo/bin/bash
HOSTNAME=variables~printing-exported-variables
PWD=/home/hacker
DOJO_AUTH_TOKEN=55347bf6f5e61075756d7e33ca7d40bac030210eb31bf3a0ae39dc63c2983542
HOME=/home/hacker
LANG=C.UTF-8
LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=00:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.7z=01;31:*.ace=01;31:*.alz=01;31:*.apk=01;31:*.arc=01;31:*.arj=01;31:*.bz=01;31:*.bz2=01;31:*.cab=01;31:*.cpio=01;31:*.crate=01;31:*.deb=01;31:*.drpm=01;31:*.dwm=01;31:*.dz=01;31:*.ear=01;31:*.egg=01;31:*.esd=01;31:*.gz=01;31:*.jar=01;31:*.lha=01;31:*.lrz=01;31:*.lz=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.lzo=01;31:*.pyz=01;31:*.rar=01;31:*.rpm=01;31:*.rz=01;31:*.sar=01;31:*.swm=01;31:*.t7z=01;31:*.tar=01;31:*.taz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tgz=01;31:*.tlz=01;31:*.txz=01;31:*.tz=01;31:*.tzo=01;31:*.tzst=01;31:*.udeb=01;31:*.war=01;31:*.whl=01;31:*.wim=01;31:*.xz=01;31:*.z=01;31:*.zip=01;31:*.zoo=01;31:*.zst=01;31:*.avif=01;35:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.webp=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:*~=00;90:*#=00;90:*.bak=00;90:*.crdownload=00;90:*.dpkg-dist=00;90:*.dpkg-new=00;90:*.dpkg-old=00;90:*.dpkg-tmp=00;90:*.old=00;90:*.orig=00;90:*.part=00;90:*.rej=00;90:*.rpmnew=00;90:*.rpmorig=00;90:*.rpmsave=00;90:*.swp=00;90:*.tmp=00;90:*.ucf-dist=00;90:*.ucf-new=00;90:*.ucf-old=00;90:FLAG=pwn.college{0nskzr_dL-HlBHRaocwRip5riZ8.dhTN1QDL5kTO0czW}
LESSCLOSE=/usr/bin/lesspipe %s %s
TERM=xterm
LESSOPEN=| /usr/bin/lesspipe %s
SHLVL=1
LC_CTYPE=C.UTF-8
PATH=/run/challenge/bin:/run/workspace/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
_=/run/workspace/bin/env
```

---

## Storing Command Output

In the course of working with the shell, you will often want to store the output of some command into a variable. Luckily, the shell makes this quite easy using something called ***Command Substitution***! Observe:

```
hacker@dojo:~$ FLAG=$(cat /flag)
hacker@dojo:~$ echo "$FLAG"
pwn.college{blahblahblah}
hacker@dojo:~$
```

> Trivia: You can also backticks instead of $(): 
FLAG=\`cat /flag\` instead of FLAG=$(cat /flag) in the example above. This is an older format, and has some disadvantages (for example, imagine if you wanted to nest command substitutions. How would you do $(cat $(find / -name flag)) with backticks?

### CTF

```
hacker@variables~storing-command-output:~$ PWN=$(/challenge/run)
Congratulations! You have read the flag into the PWN variable. Now print it out
and submit it!
hacker@variables~storing-command-output:~$ echo "$PWN"
pwn.college{EQ2Io4hWSEKqf2t7sCCjh1ICQUW.dVzN0UDL5kTO0czW}
```

---

### Reading Input

We'll start with reading input from the user (you). That's done using the aptly named `read` builtin, which reads input!

Here is an example using the `-p` argument, which lets you specify a prompt (otherwise, it would be hard for you, reading this now, to separate input from output in the example below):

```
hacker@dojo:~$ read -p "INPUT: " MY_VARIABLE
INPUT: Hello!
hacker@dojo:~$ echo "You entered: $MY_VARIABLE"
You entered: Hello!
```

Keep in mind, `read` reads data from your standard input! The first `Hello!`, above, was inputted rather than outputted. Let's try to be more explicit with that. Here, we annotated the beginning of each line with whether the line represents `INPUT` from the user or `OUTPUT` to the user:

```
 INPUT: hacker@dojo:~$ echo $MY_VARIABLE
OUTPUT:
 INPUT: hacker@dojo:~$ read MY_VARIABLE
 INPUT: Hello!
 INPUT: hacker@dojo:~$ echo "You entered: $MY_VARIABLE"
OUTPUT: You entered: Hello!
```

### CTF

```
hacker@variables~reading-input:~$ read PWN
COLLEGE
You've set the PWN variable properly! As promised, here is the flag:
pwn.college{oiGUoa2ILBukW7yxTn3QIEWcVsi.dhzN1QDL5kTO0czW}
```

---

## Reading Files

Often, when shell users want to read a file into an environment variable, they do something like:

```
hacker@dojo:~$ echo "test" > some_file
hacker@dojo:~$ VAR=$(cat some_file)
hacker@dojo:~$ echo $VAR
test
```

This works, but it represents what grouchy hackers call a **"Useless Use of Cat"**. That is, running a whole other program just to read the file is a waste. It turns out that you can just use the powers of the shell!

Previously, you `read` user input into a variable. You've also previously redirected files into command input! Put them together, and you can read files with the shell.

```
hacker@dojo:~$ echo "test" > some_file
hacker@dojo:~$ read VAR < some_file
hacker@dojo:~$ echo $VAR
test
```

What happened there? The example redirects `some_file` into the standard input of `read`, and so when read reads into `VAR`, it reads from the file!


### CTF

```
hacker@variables~reading-files:~$ read PWN < /challenge/read_me
You've set the PWN variable properly! As promised, here is the flag:
pwn.college{wCsfNqANOG6bjrPCXPs8zzwc2Xm.dBjM4QDL5kTO0czW}
```

---