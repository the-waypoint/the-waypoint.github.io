---
---

The file system is a hierachy of nested files. At the top, we have the root folder and cascading downwards are the rest of the files. The root folder is denoted as `/`.

When changing from one directory to another, we can use absolute or relative paths. Absolute pathing means you start at the root. Relative pathing takes into context the directory you are already in.

## Differences between Linux and Windows operating systems

1. Storage. Windows - drive letters represent different storage devices; Linux storage devices are represented into the file system. In this way, these devices are made "invisible" and essential parts of the file system can be on completely different computers and users would be none the wiser
2. Windows - backslash. Linux - forwardslash.
3. Extensions in Linux are not essential. Executables are determined by file permissions!
4. Every file has permissions in Linux affording for greater security. Historically Windows OS did not have this as it was built for single users whereas Linux was developed globally.
 
## The Root

* /bin - look up for common linux commands
* /boot
* /dev - contains files representing access points for devices: tty, fd, hd, sd, ram, cd
* _/etc_ - administration config files
* /home - directories assigned to each user login
* /media - location for automounting devices
* /lib
* /mnt
* /misc
* /opt - add-on application software
* /proc - information about system resources
* /root - root's home directory (above user directory for security)
* _/sbin_ - administrative commands and daemon processes
* /tmp - temp files used by applications
* /usr - commands, user documentation, X11 graphical files, files not required for boot process
* /var - various: server files, log files, spool files


## Chronological

* `echo "There are $(ls | wc -w) files in this directory."`
* `echo "There are $(ls -d */ | wc -w)" subdirectories in this directory`

File Types
* `ls -l`
  * (d) directory - a file that is a list / stack of files
  * (l) link - a system that allows the visibility of a particular file in multiple parts of the file tree
  * (p) pipe - a system that allows inter-process communication
  * (-) regular - a normal file
  * (s) socket - a special file that provides inter-process networking
  * (c) special - a mechanism used for input and output
* `ls -t` lists by most recently modified
* `ls -F` attaches an identifier to the files: / for directories, @ for links, * to executables
* `ls --hide=<regex>` 
* `ls -S` lists by size


Matching
* Like regular expressions:
  * `*` as many characters
  * `?` one character
  * `[]` range field, can include several characters or two characters with a dash in between
* `[abg]*` matches a file with first letter either a, b or g and as many characters thereafter
* `[a-g]*` matches a file with first letter either a, b, c, d, e, f, g and as many characters thereafter

Redirection
* Interpretation: standard output vs contents
* `<` is the default (silent) redirection of file contents to a command; `ls < $HOME`
* `>` directs standard output of command into a file, overwriting its contents
* `>>` directs standardoutput of a command to the end of the file
* `<<` initialises a 'here document' where the << is followed by a word. The word acts as open and close tags and as much text can be typed in between it till the tag is closed. Tricky to use... seems like you need to use the `bin/ed` command in conjunction with it.

Navigating Directories
* The `pwd` command draws its value from `$PWD`.
* But there is also a `$OLDPWD` which remembers the previous working director you were in: this enables you to use `cd -`!! Good for jumping.

Permissions
* `s` Execute permissions can be replaced with user or group attribution for running programs. That is, the process can be executed by anyone but the process is attributed to either a user or a group. Federal headship??
* `t` Sticky bit for directories allow others to add files to the directory but they are not allowed to remove it
* `+` Extended attributes are attached. SELinux, Access control lists etc

* 9 bits (512) are assigned to a file for permissions!! -rwxrwxrwx
* r=4, w=2, x=1
* u=user, g=group, o=other, a=all; used with +, -, =
* Numbers are quick but absolute. Useful for single files in chmod or umask.
* Letters are slower but selective. Useful for recursive chmod as this only deducts instead of overwrites

* umask is set to 0002 by default (first 0 is silent...)
* directory standard is 777
* file standard is 666 since execute is taken off for regular files (you don't execute text files)
* the .bashrc file needs to be updated to make the umask setting permanent. Seems like all shell commands are transient. To have permanence the .bashrc file must be updated.

Ownership
* Where permissions determines the rwx capabilities of a user, group or other, ownership changes who and of what group the file belongs to.
* Ownership may only be modified by the root user.
* `chown user:group <file destination>`
* `chown joe file` sets the file's user to joe.
* `chown joe:intelligence file` sets the file's user to be joe and the group to be intelligence
* Need to create users and groups in memory though otherwise you cannot assign these things

Checking for Aliases
* Commonly used commands are probably not in vanilla form. They probably have some flags attached to them by default. It is possible to check them with `alias <command>`
* `alias ls` results in `alias ls='ls --color=auto'` which is really helpful since it colour codes files of different types
* `alias mv` results in `alias mv='mv -i'` which alerts you if you are about to do a destructive task. This is because the `mv` command automatically overwrites files if they are of the same name / destination.

Copying
* Copying files from one location means new timestamps are applied to the files; the permissions is determin by the umask. However, by using the `cp -a` flag we archive the copy, meaning that they maintain / retain both their timestamps and their permissions.

Moving
* Moving transfers a file from Point A to Point B, ie from source to destination. If the location of the destination contains the exact same file name as the file coming in from Point A then a backup of the file being overwritten will be backed up.

