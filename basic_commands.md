---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# Basic Commands
siriuskoan

---

# You Should Already Know
- `cd` - Change directory
- `rm` - Remove
- `mkdir` - Create directory
- `touch` - Create file
- `cp` - Copy
- `cat` - Get the content of a file and **more things**
- `mv` - Move or rename
- `pwd` - Get current (working) directory
- `less` - A powerful file viewer
- `man` - Show mannual page
- `which` - Get executable file path

<!-- cat more things -->

---

# Outline
- `ls`
- `ln`
- `chmod`
- `grep`
- `ssh`
- `su` / `sudo`
- `scp` / `rsync`
- `ps` / `top` / `htop`
- `du` / `df`
- Introduction to Shell Script

---
layout: cover
background: none
---

# Before we start,
# Let's learn some tools.

---

# apt

A package management tool.

- `apt update`
- `apt search <keyword>`
- `apt install <package name>`
- `apt upgrade [package name]`
- `apt remove <package name>`
- `apt autoremove`

<!-- apt-get is old, now apt is recommended, but apt-get can help you to do more low-level things -->

---

# vim
**The best editor**

Two modes that are usually used:
- normal
- insert

When in normal mode, press `i` to enter insert mode.

When in insert mode, press `Esc` to enter normal mode.

---
layout: two-cols
---

# vim

::left::

Some commands that can be used in normal mode
- `dd` - cut the current line
- `yy` - copy the current line
- `p` - paste
- `x` - cut the current char
- `gg` - go to the head of the file
- `G` - go the the end of the file
- `gg=G` - reindent the whole file
- `u` - undo
- `Ctrl + R` - redo

::right::

When in normal mode, type
- `:q` to quit
- `:q!` to quit without saving
- `:w` to write
- `:x` to write and quit
- `!{shell command}` to execute shell command

::end::

[Vim Cheat Sheet](https://vim.rtorr.com/)

---

# tmux

> Tmux is a terminal multiplexer, it enables a number of terminals to be created, accessed, and controlled from a single screen.
> 
> *Linux mannual page*

The tool can help you manage multiple terminal.

You can detach the session so that the session will keep running even if your SSH connection is closed.

It also has many great features such as screen spliting, screen syncing, customization and so on.

---
layout: iframe
url: https://www.youtube.com/embed/3kxAfNDQLQw
---
---
layout: two-cols
---

# tmux

::left::

- Session
    - You get a new session when you execute tmux every time.
- Window
    - You can create multiple windows in a session.
- Pane
    - You can get a pane by spliting a window.

![tmux](/tmux-ctrl-b-w.png)

::right::

![tmux](/tmux.png)

---

# tmux
Prefix: `Ctrl + b`

- `c` - create new window
- `d` - detach current session
- `n` - switch to next window
- `p` - switch to previous window
- `{number}` - switch to speficied window
- `w` - show a list of the window to switch to
- `&` - close current window
- `"` - split current window horizontally
- `%` - split current window vertically
- `{arrow key}` - switch among panes

[Linux tmux 終端機管理工具使用教學](https://blog.gtwang.org/linux/linux-tmux-terminal-multiplexer-tutorial/)

---

# IO Redirection

- `>`, `>>`
- `<`, `<<`
- `|`

[Linux I/O 輸入與輸出重新導向](https://blog.gtwang.org/linux/linux-io-input-output-redirection-operators/)

<!--
Final step before entering commands

In C, you should include stdio, which represents standard input/output
-->

---

# IO Redirection
- `ls > ls.txt`
- `cat < input.txt`
- `ls > ls.txt 2> err.txt`
- `ls > ls.txt 2>&1`
- `ls | grep *.log`
- `cat test.txt | tail -n 20 | sort | uniq | nl`
- `cat test.txt |& my_script.sh`


<!-- Some examples here -->

---
layout: two-cols
---

# IO Redirection

::left::

```
cat << EOF | grep ls | uniq > test 2>&1
ls
grep
ls
ps
EOF
```

The result is

```
ls
```

::right::

```
cat << EOF > out.txt
test
test2
test3
EOF
```

It will generate a file containing the three lines.

<!-- Some cat examples here -->

---
layout: cover
background: none
---

# OK, let's get started!

---

# `ls`
First thing to know: everything you see in Linux is a file, i.e.,
- Directory is file
- Link is file
- Your screen is file
- Your keyboard is file
- ...

---

# `ls`
Two types of path:
- absolute path (start with `/`): `/var/log/auth.log`
- relative path (start from current directory): `test/meow.txt`

Some special path
- `.` - current directory
- `..` - parent directory
- `~` - home directory
- `/` - root directory

<!-- path types -->

---

# `ls`
`ls -l` (or `ll`)

```
-rw-rw----    1 siriuskoan wheel     217 Apr 8 14:08 test
```

<!--

permission hard_link owner group size filename

`ll` is alias, and not all shell has this alias

-->

---

# `ls`
File types:
- `-`: Regular file
- `b`: Block device file
- `c`: Character device file
- `d`: Directory
- `l`: Symbolic link
- `s`: Unix domain socket
- `p`: Named pipe

Use `file` command to determine its file type.

If you are interested in how to make a character device or block device, check [this](https://hackmd.io/@combo-tw/ryRp--nQS).

<!--
file types

block device: IO with block size, like disk  
character device: a stream of characters, like keyboard  
socket: a way to communicate with other processes  
named pipe: just like a pipe, but it has name, you can pass some value to it by one process and get it by another process
-->

---

# `ln`
> `ln` - make links between files
>
> *Linux mannual page*

There are two types of links
- hard: another entrypoint of file
- symbolic (soft): just like "shortcut" in Windows

To create hard link, use `ln [original_file] [hard_link]`

To create soft link, use `ln -s [original_file] [soft_link]`

<!-- the difference between hard and soft link and commands -->

---

# `ln`
Every file has its own inode. Use `ls -i` to view the inode of file.

A hard link means the same inode as original file, but a soft link has different one.

The data block and inode will be taken back by OS once all hard links are deleted.

When the system says "no more space", it may mean you don't have space for another new inode.

<!-- knowledge about inode -->

---

# `chmod`
Recall the information shown in `ll`.

```
-rw-rw----    1 siriuskoan wheel     217 Apr 8 14:08 test
```

- `r`: read
- `w`: write
- `x`: execute

For directory, `rwx` represent "can `ls` in it", "can write a file in it" and "can `cd` into it".

Every three characters shows permissions of a class of user, which are owner, user in the group, others, respectively.

---

# `chmod`

![](/rwx.jpg)

---

# `chmod`

Sometimes, the permissions not only contain `r`, `w` and `x` but also contain `s`, which means setuid or setgid (depending on where the `s` is).

They allow users to execute a file as its owner or as people in its group.

- A file with permissions `rwsr-xr-x`.

  A user who is classified as "others" can execute the file, and due to the `s` in owner permissions, when the user execute the file, he or she will become its owner.

- A file with permissions `rwxrwsr-x`.

  A user who is classified as "others" can execute the file, and due to the `s` in group permissions, when the user execute the file, he or she will become a member in its group.

A real example is `/user/bin/passwd`: `-rwsr-xr-x`.

<!-- Everyone can change their own password, but changing password means modifying system config file, so we need setuid to grant permissions to normal user -->

---

# `chmod`

- uid - who runs the script
- gid - the group the runner belongs to
- euid - who actually runs the script and acquires the resources
- egid - the group which actually runs the script and acquires the resources

In normal situation, `uid == euid`, `gid == egid`.

However, if we use `setuid` and `setgid`, `euid` becomes the owner's id and `egid` becomes the group's id.

---

# `chmod`

We can use this simple program to check the uid, gid, euid and egid.

```c
#include <stdio.h>
#include <unistd.h>
int main() {
    uid_t real_uid = getuid();
    uid_t effect_uid = geteuid();
    gid_t real_gid = getgid();
    gid_t effect_gid = getegid();
    printf("ruid=%d, euid=%d\n", real_uid, effect_uid);
    printf("rgid=%d, egid=%d\n", real_gid, effect_gid);
    return 0;
}
```

---

# `chmod`

Sometimes, there is `t` in permissions. It means sticky bit.

Sticky bit means only the owner of a file (and superuser) can remove or move it.

A real example is `/tmp`: `drwxrwxrwt`.

---

# `chmod`
There are two ways to change permission.

- The first way is `chmod [0-4][0-7]{3} test`.

  In the first set of number, `4`, `2`, `1` represents setuid, setgid, sticky bit, respectively. This part can be omitted.

  In the second set of number, `4`, `2`, `1` represents `r`, `w`, `x`, respectively.

  For example, `chmod 1777 test`.

<!-- 4, 2, 1 are 100, 010, 001 in binary -->

---

# `chmod`

- The second way is `chmod [ugoa][+-=][srwx] test`

  `u`, `g`, `o`, `a` represents user (owner), group, others, all, respectively.

  `+`, `-`, `=` represents grant, deprive, set, respectively

  If there are multiple settings you want to change, you can use comma to separate them.

  For example, `chmod uo+rx,ug+w test`

  To add or remove sticky bit is a little bit different, it only requires `+-` and `t`.
  For example, `chmod +t test`.

---

# `chmod`

In most of the cases, we use `-r` option to recursively apply something.

However, we should use `-R` option to accomplish this.

`chmod -R 777 d/` will change the permission of all the files and directories under `d/` to `777`.

It is the same when we use `chown` and `chgrp`.

---

# `grep`

> grep - print lines that match patterns
>
> *Linux manual page*

It is a very useful tool when checking log.

patterns: regular expression

<!-- basic knowledge about regex -->

---
layout: two-cols
---

# `grep`

::left::

Some common options
- `-i` - ignore case different
- `-r` - recursively
- `-n` - show line number
- `-f` - read from file
- `-e` - regex patterns
- `-v` - logical NOT
- `-A` - print some lines after the matched lines
- `-B` - print some lines before the matched lines
- `-C` - print some lines before and after the matched lines

::right::

Some common usage
- `cat test.txt | grep test`
- `grep "test" test.txt`
- `grep -inr "test" .`
- `grep -A 2 -B 2 "test" test.txt`
- `grep -C 2 "test" test.txt` (the same output as the above one)

---

# `ssh`

SSH, standing for Secure Shell, is a protocol that provides secure channel for transferring.

Two ways to connect
- Use `ssh [username]@[hostname or IP address]` to connect to remote server and start your work.

- Use `ssh-keygen` to generate public key and private key, and use `ssh-copy-id` to copy your key to remote server.

In this way, no password is required.

<!--
we will not talk about crypto, just about how to use it, but you should know about key.

In this way, we need password, use key to get rid of it.

steps:

`ssh-keygen`

`ssh-copy-id [hostname]`

(If you leave everything default)
-->

---

# `su` / `sudo`

Both of them make you have higher privileges, but there are still some differences between them.
- `su` gives you full permissions of root. When using it, you should use **root password**.
- `sudo` gives you temporary higher permissions for executing certain commands. When using it, you should use **your own password**.

![sudo](/sudo_meme.jpg)

---

# `su` / `sudo`

Using `sudo` is much better than using `su` when a team maintains a system. The reasons are
- It can give different team members different permissions, i.e., your system can have better access control.
- Every command executed with `sudo` will be recorded in `/var/log/secure`, so if there is something wrong, it is easy to find out who did it.

We should not do anything like `bash` with `sudo` since others cannot know what you do.

That is, we should not do anything that (can) creates a shell with `sudo`. For example, `su`, `sh`, `vim`, `less` and so on.

<!--
To edit, you can use rvim or edit

To read, you can use `sudo cat log_file | less` since sudo can apply to cat command.
-->

---

# `su` / `sudo`

To run command as root with `su`
- `su` - switch to root
- `su -` - login as root

To switch to another user, we can use `su - [user]`.

<!--
If you only use `su`, some env variables may be not the same as root, causing "command not found" error.

One difference is that using `su` you will stay at your current path, but using `su -` you will move to home directory of root.
-->

---

# `su` / `sudo`

To run command as others with `sudo`
- `sudo [command]` - using `sudo` to run a command as root
- `sudo !!` - using `sudo` to run last command as root
- `sudo -u [user] [command]` - using `sudo` to run command as specified user
- `sudo -g [group] [command]` - using `sudo` to run command as specified group

![sudo !!](/sudo_last.jpg)

---

# `su` / `sudo`

We can use `visudo` to edit sudo config file (`/etc/sudoers`).

```
User Host=(RunAsUser:Group) Commands
```

For example,
- `root ALL = (ALL:ALL) ALL`
- `%sudo ALL = (ALL:ALL) ALL`
- `siriuskoan ALL = (root) /bin/cat,/bin/ls`
- `siriuskoan ALL = (root) NOPASSWD:ALL`

[Linux Fundamentals: A to Z of a Sudoers File.](https://medium.com/kernel-space/linux-fundamentals-a-to-z-of-a-sudoers-file-a5da99a30e7f)

[sudoers(5) - Linux man page](https://linux.die.net/man/5/sudoers)

<!-- There is alias in sudoers, but we don't talk about that -->

---

# `su` / `sudo`

Some common errors
- `[username] is not in the Sudoers file. This incident will be reported.`
- `Sorry, user [username] is not allowed to execute [command] as [user] on [hostname]`

![not in](/sudo_meme_2.jpg)

---

# `scp` / `rsync`

> `scp` - secure copy (remote file copy program)
>
> *Linux manual page*

`scp` uses SSH protocol to copy file from one host to another.

`scp` is a good friend when we need to copy file from one host to another host. However, `scp` has been deprecated, we should use `rsync` instead.

---

# `scp` / `rsync`

Usage
- `scp [path] [user]@[remote host]:[path]` - copy file to remote host
- `scp [user]@[remote host]:[path] [path]` - copy file from remote host

Examples
- `scp ~/test.txt siriuskoan@my-host:~`
- `scp phkoan@linux1.cs.nctu.edu.tw:~/test.txt ~`

Options
- `-r` - recursively
- `-p` - preserves modification times, access times, and modes from the original file.
- `-C` - enable compression

[scp 指令用法教學與範例](https://blog.gtwang.org/linux/linux-scp-command-tutorial-examples/)


<!-- Remember to add `:[path]`, or it will copy `test.txt` to `siriuskoan@my-host`, which is a filename -->

---

# `scp` / `rsync`

> `rsync` - a fast, versatile, remote (and local) file-copying tool
>
> *Linux manual page*

`rsync` can do what `cp` and `scp` do, moreover, it is more efficient.

Advantages
- Speed
- Better security
- Delta transfer algorithm

---

# `scp` / `rsync`

Examples
- `rsync ~/test.txt  siriuskoan@my-host:~`
- `rsync phkoan@linux1.cs.nctu.edu.tw:~/test.txt ~`

Options
- `-r` - recursively
- `-a` - archive
- `-z` - enable compression
- `--delete` - delete receiving side file if it does not exist in the sending files
- `--progress` - show progress

[使用 rsync 遠端檔案同步與備份工具教學與範例](https://blog.gtwang.org/linux/rsync-local-remote-file-synchronization-commands/)

<!--
Usage just like `scp`
-->

---

# `ps` / `top` / `htop`

They are process and system monitoring tools.

> ps - report a snapshot of the current processes
>
> *Linux manual page*

> top - display Linux processes
>
> *Linux manual page*

> htop - interactive process viewer
>
> *Linux manual page*

---

# `ps` / `top` / `htop`

`ps aux` is a command we often use.

![ps](/ps.png)

<!--

USER: user

PID: process ID, every process has identical one. PIDs are serial numbers.

VSZ: Virtual Memory Size

RSS: Resident Set Size

https://linuxconfig.org/ps-output-difference-between-vsz-vs-rss-memory-usage

TTY: terminal device, like your screen. When you ssh, the terminal is pty

STAT: state, `I` means idle, `S` means sleeping, the second letter is additional flag

-->

---

# `ps` / `top` / `htop`

Program is dead, when you execute it, it becomes a process.

Attributes of the processes
- PID, PPID - Process ID and Parent PID
- UID, EUID - User ID and Effective UID
- GID, EGID - Group ID and Effective GID
- Niceness - Priority

<!--

We have talked about UID, EUID, GID and EGID before.

PPID is the Parent PID, which manifests which process create this process, we can check this out with htop

Niceness is means the priority, the higher the niceness is, the lower priority the process has. We can use `nice` or `renice` to update the value

-->

---

# `ps` / `top` / `htop`

`top` and `htop` are both process viewers, we can use them to check out some system and processes information.

`htop` is a better process viewer than `top`.

Let's see how to use them.

<!--

Just open terminal and show `top` and `htop`

-->

---

# `du` / `df`

> du - estimate file space usage
>
> *Linux manual page*

> df - report file system disk space usage
>
> *Linux manual page*

---

# `du` / `df`

They are some tools that help us check disk usage or file / directory size.

Usage
- `du -ah ~` - show the size of all files under home directory
- `du -ah -d 1 /` - show the size of all directories under root directory (because of `-d 1`)
- `du -hs ~` - show how large the home directory is
- `df -Th` - show all filesystems disk usage along with their filesystem types

<!--

`-a` means all files, not only directories

`-d` means depth

`-h` means human-readable

`-s` means the same as `-d 0`

`-T` means filesystem type

-->

---

# Introduction to Shell Script

Shell is a bridge between user and system kernel.

There are many shells such as Bourne shell (sh), bash, zsh and so on.

![](/shell.png)

<!--

https://medium.com/@truong21/what-happens-where-you-type-ls-l-in-a-linux-shell-a-command-line-journey-7f4dbf051336

-->

---

# Introduction to Shell Script

By writting shell script, several things like analyzing log and user management can be done automatically.

We can program in shell language, which has variables, flow control, input, output and so on, just like all the other languages, but it has its own syntax.

Shell language is a interpreted language, it can be run directly in your shell.

Actually, shell script consists of the commands we use, but with some important components such as special variables, exit code and so on.

After finishing your script, remember to `chmod` it to allow users to execute it.

<!--

The commands just like functions `print`, `int` in Python.

There are many things to be understood in shell script, but we cannot talk about them due to limit of time, even the syntax is skipped.

Shebang is not so easy, and the shell location may differ from OS from OS, you should be careful.

-->

---

# Introduction to Shell Script

Shell script file starts with "Shebang" (`#!`), the sign specifies which program is used to run it.

For example, if we want `bash` to run the script, the Shebang is `#!/bin/bash`.

Also, we can use `python` to run it, and the Shebang is `#!/usr/bin/python`.

Then, you can run it by executing command `./your_script_name.sh` (be sure you have make it executable).

---

# Introduction to Shell Script

Let's see an example of shell script.

```bash
#!/bin/bash

my_name="$1";

if [[ ${my_name} == "siriuskoan" ]]; then
    echo "You are admin!";
else
    echo "You are not admin, the incident will be reported.";
    echo "${my_name} attempted to run this script" > /root/my_script.log;
fi
```

<!--

`$1` is the first argument from command line.

When assigning variables, `$` is not needed, but when accessing variables, it is needed.

`fi` is the end of `if`.

-->

---

# Introduction to Shell Script

If you want to know more about the syntax of shell script, you can check out the following two websites.

[NCTU SA - Shell Programming](https://nasa.cs.nctu.edu.tw/sa/2021/slides/04_ShellProgramming.pdf)

[Bash scripting cheatsheet](https://devhints.io/bash)

<!--

The first one is the slides from the NCTU class called SA.

The second one is cheatsheet.

If much time is left, open the slides.

-->
