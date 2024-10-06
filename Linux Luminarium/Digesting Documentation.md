# Digesting Documentation

[Resources](https://man7.org/linux/man-pages/)

---

## Learning From Documentation

The correct usage of programs depends, in a large part, in the proper specification of arguments to them. 
Recall the `-a` of `ls -a` in the ***hidden files*** challenge of the **Basic Commands** module: that `-a` was an argument that told `ls` to list out hidden files as well as non-hidden files. 
Because we wanted to list out hidden files, invoking `ls` with the `-a` argument was the correct way to use it in our scenario.

### CTF

```
hacker@man~learning-from-documentation:~$ /challenge/challenge --giveflag
Correct argument! Here is your flag:
pwn.college{0nIfcQTBTnFFc9nHVPTB_Djz2eq.dRjM5QDL5kTO0czW}
```

---

## Learning Complex usage

### CTF

```
hacker@man~learning-complex-usage:~$ /challenge/challenge --printfile /flag
Correct argument! Here is the /flag file:
pwn.college{kUyYgKEoNOeMHdjJI9mt2ZtFTjF.dVjM5QDL5kTO0czW}
```

---

## Reading Manuals

`man` is short for **manual**, and will display (if available) the manual of the command you pass as an argument. 
For example, let's say we wanted to learn about the `yes` command (yes, this is a real command):

`hacker@dojo:~$ man yes`

This will display the manual page for `yes`, which will look something like this:

```
YES(1)                           User Commands                          YES(1)

NAME
       yes - output a string repeatedly until killed

SYNOPSIS
       yes [STRING]...
       yes OPTION

DESCRIPTION
       Repeatedly output a line with all specified STRING(s), or 'y'.

       --help display this help and exit

       --version
              output version information and exit

AUTHOR
       Written by David MacKenzie.

REPORTING BUGS
       GNU coreutils online help: <https://www.gnu.org/software/coreutils/>
       Report any translation bugs to <https://translationproject.org/team/>

COPYRIGHT
       Copyright  ©  2020  Free Software Foundation, Inc.  License GPLv3+: GNU
       GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
       This is free software: you are free  to  change  and  redistribute  it.
       There is NO WARRANTY, to the extent permitted by law.

SEE ALSO
       Full documentation <https://www.gnu.org/software/coreutils/yes>
       or available locally via: info '(coreutils) yes invocation'

GNU coreutils 8.32               February 2022                          YES(1)
```

The important sections are:

```
NAME(1)                           CATEGORY                          NAME(1)

NAME
	This gives the name (and short description) of the command or
	concept discussed by the page.

SYNOPSIS
	This gives a short usage synopsis. These synopses have a standard
	format. Typically, each line is a different valid invocation of the
	command, and the lines can be read as:

	COMMAND [OPTIONAL_ARGUMENT] SINGLE_MANDATORY_ARGUMENT
	COMMAND [OPTIONAL_ARGUMENT] MULTIPLE_ARGUMENTS...

DESCRIPTION
	Details of the command or concept, with detailed descriptions
	of the various options.

SEE ALSO
	Other man pages or online resources that might be useful.

COLLECTION                        DATE                          NAME(1)
```

You can scroll around the manpage with your arrow keys and PgUp/PgDn.
When you're done reading the manpage, you can hit `q` to quit.

Manpages are stored in a centralized database. 
If you're curious, this database lives in the `/usr/share/man` directory, but you never need to interact with it directly: you just query it using the `man` command. 
The arguments to the `man` command aren't file paths, but just the names of the entries themselves 
(e.g., you run `man yes` to look up the `yes` manpage, rather than `man /usr/bin/yes`, which would be the actual path to the `yes` program but would result in `man` displaying garbage).

### CTF

```
hacker@man~reading-manuals:~$ man challenge

CHALLENGE(1)                  Challenge Commands                  CHALLENGE(1)

NAME
       /challenge/challenge - print the flag!

SYNOPSIS
       challenge OPTION

DESCRIPTION
       Output the flag when called with the right arguments.

       --fortune
              read a fortune

       --version
              output version information and exit

       --monjfr NUM
              print the flag if NUM is 727

AUTHOR
       Written by Zardus.
hacker@man~reading-manuals:~$ /challenge/challenge --monjfr 727
Correct usage! Your flag: pwn.college{UmBonFOQCCjA_7frML279I2KOW4.dRTM4QDL5kTO0czW}
```

---

## Searching Manuals

You can scroll man pages with the arrow keys (and PgUp/PgDn) and search with `/`. After searching, you can hit `n` to go to the next result and `N` to go to the previous result. Instead of `/`, you can use `?` to search backwards!

### CTF

```
hacker@man~searching-manuals:~$ man challenge
hacker@man~searching-manuals:~$ /challenge/challenge --yxmbz
Initializing...
Correct usage! Your flag: pwn.college{sJFyvmLXJVG-d0urusUj09KVLvx.dVTM4QDL5kTO0czW}
```

---

## Searching For Manuals

### CTF

```
hacker@man~searching-for-manuals:~$ man man
MAN(1)                                                                                    Manual pager utils                                                                                    MAN(1)

NAME
       man - an interface to the system reference manuals

SYNOPSIS
       man [man options] [[section] page ...] ...
       man -k [apropos options] regexp ...
       man -K [man options] [section] term ...
       man -f [whatis options] page ...
       man -l [man options] file ...
       man -w|-W [man options] page ...

DESCRIPTION
       man  is  the  system's  manual pager.  Each page argument given to man is normally the name of a program, utility or function.  The manual page associated with each of these arguments is then
       found and displayed.  A section, if provided, will direct man to look only in that section of the manual.  The default action is to search in all of the available sections following a pre-de‐
       fined order (see DEFAULTS), and to show only the first page found, even if page exists in several sections.

       The table below shows the section numbers of the manual followed by the types of pages they contain.

       1   Executable programs or shell commands
       2   System calls (functions provided by the kernel)
       3   Library calls (functions within program libraries)
       4   Special files (usually found in /dev)
       5   File formats and conventions, e.g. /etc/passwd
       6   Games
       7   Miscellaneous (including macro packages and conventions), e.g. man(7), groff(7)
       8   System administration commands (usually only for root)
       9   Kernel routines [Non standard]

       A manual page consists of several sections.

       Conventional  section  names include NAME, SYNOPSIS, CONFIGURATION, DESCRIPTION, OPTIONS, EXIT STATUS, RETURN VALUE, ERRORS, ENVIRONMENT, FILES, VERSIONS, CONFORMING TO, NOTES, BUGS, EXAMPLE,
       AUTHORS, and SEE ALSO.

       The following conventions apply to the SYNOPSIS section and can be used as a guide in other sections.

       bold text          type exactly as shown.
       italic text        replace with appropriate argument.
       [-abc]             any or all arguments within [ ] are optional.
       -a|-b              options delimited by | cannot be used together.
       argument ...       argument is repeatable.
       [expression] ...   entire expression within [ ] is repeatable.

       Exact rendering may vary depending on the output device.  For instance, man will usually not be able to render italics when running in  a  terminal,  and  will  typically  use  underlined  or
       coloured text instead.

       The  command  or  function  illustration is a pattern that should match all possible invocations.  In some cases it is advisable to illustrate several exclusive invocations as is shown in the
       SYNOPSIS section of this manual page.

EXAMPLES
       man ls
           Display the manual page for the item (program) ls.

       man man.7
           Display the manual page for macro package man from section 7.  (This is an alternative spelling of "man 7 man".)

       man 'man(7)'
           Display the manual page for macro package man from section 7.  (This is another alternative spelling of "man 7 man".  It may be more convenient when copying and  pasting  cross-references
           to manual pages.  Note that the parentheses must normally be quoted to protect them from the shell.)

       man -a intro
           Display, in succession, all of the available intro manual pages contained within the manual.  It is possible to quit between successive displays or skip any of them.

       man -t bash | lpr -Pps
           Format  the manual page for bash into the default troff or groff format and pipe it to the printer named ps.  The default output for groff is usually PostScript.  man --help should advise
           as to which processor is bound to the -t option.

       man -l -Tdvi ./foo.1x.gz > ./foo.1x.dvi
           This command will decompress and format the nroff source manual page ./foo.1x.gz into a device independent (dvi) file.  The redirection is necessary as the -T flag causes output to be di‐
           rected to stdout with no pager.  The output could be viewed with a program such as xdvi or further processed into PostScript using a program such as dvips.

       man -k printf
           Search the short descriptions and manual page names for the keyword printf as regular expression.  Print out any matches.  Equivalent to apropos printf.

       man -f smail
           Lookup the manual pages referenced by smail and print out the short descriptions of any found.  Equivalent to whatis smail.
hacker@man~searching-for-manuals:~$ man -k challenge
hmolvnsvur (1)       - print the flag!
hacker@man~searching-for-manuals:~$ man hmolvnsvur

CHALLENGE(1)                                                                              Challenge Commands                                                                              CHALLENGE(1)

NAME
       /challenge/challenge - print the flag!

SYNOPSIS
       challenge OPTION

DESCRIPTION
       Output the flag when called with the right arguments.

       --fortune
              read a fortune

       --version
              output version information and exit

       --hmolvn NUM
              print the flag if NUM is 044

AUTHOR
       Written by Zardus.

REPORTING BUGS
       The repository for this dojo: <https://github.com/pwncollege/linux-luminarium/>

SEE ALSO
       man(1) bash-builtins(7)

pwn.college                                                                                    May 2024                                                                                   CHALLENGE(1)
hacker@man~searching-for-manuals:~$ /challenge/challenge --hmolvn 044
Correct usage! Your flag: pwn.college{0hmoDZTlYvOQUnCLsv4uPZrCCEM.dZTM4QDL5kTO0czW}
```

---

## Helpful Programs

Some programs don't have a man page, but might tell you how to run them if invoked with a special argument. 
Usually, this argument is `--help`, but it can often be `-h` or, in rare cases, `-?`, `help`, or other esoteric values like `/?` (though that latter is more frequently encountered on Windows).

### CTF

```
hacker@man~helpful-programs:/$ /challenge/challenge --help
usage: a challenge to make you ask for help [-h] [--fortune] [-v]
                                            [-g GIVE_THE_FLAG] [-p]

optional arguments:
  -h, --help            show this help message and exit
  --fortune             read your fortune
  -v, --version         get the version number
  -g GIVE_THE_FLAG, --give-the-flag GIVE_THE_FLAG
                        get the flag, if given the correct value
  -p, --print-value     print the value that will cause the -g option to give
                        you the flag
hacker@man~helpful-programs:/$ /challenge/challenge -p
The secret value is: 824
hacker@man~helpful-programs:/$ /challenge/challenge -g 824
Correct usage! Your flag: pwn.college{8KZSlAMalxF2DZXxxFmxibbaCll.ddjM4QDL5kTO0czW}
```

---

## Help for builtins

Some commands, rather than being programs with man pages and help options, are built into the shell itself. 
These are called ***builtins***. 
Builtins are invoked just like commands, but the shell handles them internally instead of launching other programs. 
You can get a list of shell builtins by running the ***builtin*** `help`, as so:

`hacker@dojo:~$ help`

You can get help on a specific one by passing it to the `help` builtin.
Let's look at a builtin that we've already used earlier, `cd`!

```
hacker@dojo:~$ help cd
cd: cd [-L|[-P [-e]] [-@]] [dir]
    Change the shell working directory.
    
    Change the current directory to DIR.  The default DIR is the value of the
    HOME shell variable.
...
```

### CTF

```
hacker@man~help-for-builtins:~$ help challenge
challenge: challenge [--fortune] [--version] [--secret SECRET]
    This builtin command will read you the flag, given the right arguments!
    
    Options:
      --fortune		display a fortune
      --version		display the version
      --secret VALUE	prints the flag, if VALUE is correct

    You must be sure to provide the right value to --secret. That value
    is "0MFcfQTe".
hacker@man~help-for-builtins:~$ challenge --secret  0MFcfQTe
Correct! Here is your flag!
pwn.college{0MFcfQTeUfe5HElLp6ijxPobSSm.dRTM5QDL5kTO0czW}
```

---

