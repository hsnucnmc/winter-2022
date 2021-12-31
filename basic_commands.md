---
theme: seriph
download: true
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
- `cd` - change directory
- `rm` - remove
- `mkdir` - create directory
- `touch` - create file
- `cp` - copy
- `cat` - get the content of a file and more things
- `mv` - move or rename
- `pwd` - get current (working) directory
- `less` - a powerful file viewer
- `man` - show mannual page
- `which` - get executable file path

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

But before we start, let's learn some tools.

---

# apt

A package management tool.

- `apt update`
- `apt search {keyword}`
- `apt install {package name}`
- `apt upgrade [package name]`
- `apt remove {package name}`
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

https://vim.rtorr.com/

---

# tmux

> Tmux is a terminal multiplexer, it enables a number of terminals to be created, accessed, and controlled from a single screen.
> 
> Linux mannual page

The tool can help you manage multiple terminal.

You can detach the session so that the session will keep running even if your SSH connection is closed.

It also has many great features such as screen spliting, sync screen, customization and so on.

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

https://blog.gtwang.org/linux/linux-tmux-terminal-multiplexer-tutorial/

---

# IO Redirection

- `>`, `>>`
- `<`, `<<`
- `|`

https://blog.gtwang.org/linux/linux-io-input-output-redirection-operators/

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

# IO Redirection
```
cat << EOF | grep ls | uniq > test 2>&1
ls
grep
ls
ps
EOF
```

```
cat << EOF > out.txt
test
test2
test3
EOF
```

<!-- Some cat examples here -->

---
layout: cover
background: none
---

# OK, let's get started!

---

# `ls`
First thing to know: everything you see in Linux is file, i.e.,
- Directory is file
- Link is file
- Your screen is file
- Your keyboard is file
- ...

---

# `ls`
Two types of path:
- absolute path (start with /): /var/log/auth.log
- relative path (start from current directory): test/meow.txt

<!-- path types -->

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

# `ls`
`ls -l` (or `ll`)

```
-rw-rw----    1 siriuskoan wheel     217 Apr 8 14:08 test
```

<!-- permission hard_link owner group size filename -->

---

# `ln`
> `ln` - make links between files
>
> Linux mannual page

There are two types of links
- hard: another entrypoint of file
- symbolic (soft): just like "shortcut" in Windows

To create hard link, use `ln original_file hard_link`

To create soft link, use `ln -s original_file soft_link`

<!-- the difference between hard and soft link and commands -->

---

# `ln`
Every file has its own inode. Use `ls -i` to view the inode of file.

A hard link means the same inode as original file, but a soft link has different one.

When a hard link exists, the file will exist. Otherwise, the data block and inode will be taken back by OS.

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

Every three characters shows permissions of a class of user.

Owner, user in the group, others, respectively.

---

# `chmod`

Sometimes, the permissions not only contain `r`, `w` and `x` but also contain `s`, which means setuid or setgid (depending on where the `s` is).

They allow users to execute a file as its owner or as people in its group.

- A file with permissions `rwsr-xr-x`.

  A user who is classified as "others" can execute the file, and due to the `s` in owner permissions, when the user execute the file, he or she will become its owner.

- A file with permissions `rwxrwsr-x`.

  A user who is classified as "others" can execute the file, and due to the `s` in group permissions, when the user execute the file, ge or she will become a member in its group.

A real example is `/user/bin/passwd`: `-rwsr-xr-x`.

<!-- Everyone can change their own password, but changing password means modifying system config file, so we need setuid to grant permissions to normal user -->

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
