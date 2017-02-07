---
---

## Chronological

* `whoami`
* `grep username /etc/passwd`
* `hostname`

* `pwd`
* `ls`
* `tar -cvf backup.tar /home/directory`

* [`du -sh backup.tar`](https://www.linux.com/learn/du-know-how-big-your-linux-files-are)
* `df`

* `uptime`
* `w`
* `who -uH`

* `cat`
* `less`
* `more`

* `date [+FORMAT]`
* `date +'%d/%m/%Y'`
* `echo "Today is $(date +'%A, %M %d, %Y')`


Command lookup path:
* `echo $PATH | tr : '\n'`
* `type -a ls`
* `type pwd`
* `type which`
* `type case`
* `type return`

Variables
* `echo $HOSTNAME`
* `echo $USER`
* `echo $SHELL`
* `echo $HOME`

Utilitarian:
* `^P` - 'up' arrow key equivalent
* `^N` - 'down' arrow key equivalent

* `^F` - forward one character
* `^B` - backward one character
* `_F` - forward one word
* `_B` - backward one word
* `^A` - start of line
* `^E` - end of line

* `^R` - reverse incremental search

Open files in text editor:
* `atom $(find _posts | grep .md)` - I know: sorry for being an atom plebeian

RTFM:
* `man -k <command>`
  * the -k flag helps to show you elsewhere the `<command>` is being mentioned and in what section of the manual it is being mentioned
  * synonymous with `apopros <command>` - who knew?
* `man 5 passwd`
* Main areas to look at:
  1. User Commands
  2. System Calls (functions provided by the kernal)
  3. C Library Calls (functions provided by program libraries)
  4. Devices and Special Files (found in /dev)
  5. File Formats and Conventions eg /etc/passwd
  6. Games
  7. Miscellaneous; things like protocols and file systems
  8. System Administration Tools and Daemons
* By default `man <command>` only brings up the first possible manual section for the command!!
* Navigating the `man` pages:
  * `/` to search for a term
  * `n` to search for next term
  * `N` to search for previous term

