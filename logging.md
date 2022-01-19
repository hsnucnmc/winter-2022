---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# Logging

siriuskoan

---

# Outline

- Why Do We Need Log Files
- Important Log Files
- Log Rotate
- Syslog
  - Log Level
  - Log Facility
  - Config
  - `logger` Command

---

# Why Do We Need Log Files?

- We can know what went wrong when an incident happens
- We can know who did what before an incident happens

For example,
- Mail log
- HTTP server log
- System log
- ...

<!--

All in all, log files are for post-incident tracking

-->

---

# Important Log Files

Generally, all log files are in `/var/log` directory.

- `/var/log/messages` - Almost every incident
- `/var/log/lastlog` - All login records, use `lastlog` to access
- `/var/log/faillog` - All failed login records, use `faillog` to access
- `/var/log/maillog` - Mail server log
- `/var/log/auth.log` - System authorization information (PAM log)
- `/var/log/kern.log` - Kernel log
- ...

<!--

Kernel is the core of Unix-like OS

hardware -> kernel -> shell -> you

-->

---

# Important Log Files

When we setup a service, it will create its own log files.

We can find them by reading the config file.

Take Nginx, a web server we will mention later, as example,

```nginx
##
# Logging Settings
##

access_log /var/log/nginx/access.log;
error_log /var/log/nginx/error.log;

```

---

# Log Ratate

As the time elapsed, the log files got larger and larger.

It has some disadvantages
- It's hard to open the files
- It's hard to find out what happened at when

Therefore, we have to adopt log rotate to make the log files separated.

In `/var/log`, there are many examples such as `auth.log.1`.

<!--

The log files will be rotated every day or any other time interval.

Or they can be rotated by size.

-->

---

# Syslog

Syslog is a daemon that log messages with certain level to specified log files.

Config file: `/etc/syslog.conf`, `/etc/syslog.d/*`

Command: `logger`

C library: `syslog.h`

<!--

We or our shell script can use `logger` command to log messages.

So we don't have to manually write log messages into log files.

-->

---

# Syslog - Log Level

- `EMERGENCY` - A "panic" condition
- `ALERT` - Should be corrected immediately
- `CRITICAL` - Should be corrected immediately, but indicates failure in a primary system
- `ERROR` - Non-urgent failures
- `WARNING` - Warning messages - not an error, but indication that an error will occur if action is not taken
- `NOTICE` - Events that are unusual but not error conditions - no immediate action required
- `INFO` - Normal operational messages
- `DEBUG` - Info useful to developers for debugging the app

[What are Syslog Facilities and Levels?](https://success.trendmicro.com/solution/TP000086250-What-are-Syslog-Facilities-and-Levels)

<!--

The log level manifests how serious the condition is

-->

---

# Syslog - Facilities

- `kern` - The kernel
- `user` - User processes (default)
- `mail` - `sendmail` and other mail-related software
- `daemon` - System daemons
- `auth` - Security and authorization-related commands
- `cron` - The cron daemon
- `syslog` - `syslogd` internal messages
- ...

[NCTU SA - Syslog and LogRotate](https://nasa.cs.nctu.edu.tw/sa/2021/slides/17_Syslog_and_LogRotate.pdf (P12))

<!--

The facility represents the machine process that created the syslog event.

-->

---

# Syslog - Config

Config file: `/etc/syslog.conf`, `/etc/syslog.d/*`

The config file manifests how log messages should be recorded.

The basic format is `{facility}.{level}  {action}`, `{facility}.{level}` part is also called "selector".

We can also use property-based filter. Check out [Property-Based Filters](https://www.rsyslog.com/doc/v8-stable/configuration/filters.html#property-based-filters).

<!--

While the selector provides a way to classify log messages by its source and level, property-based filter provides a way to classify them by its content.

-->

---

# Syslog - Config

We can also combine many selectors by `,` or `;`.

```systemd
{facility1},{facility2}.{level} {action}
{facility1}.{level};{facility2}.{level} {action}
```

We can also use `*` to represents all facilities or levels.

You may prefix each entry with the minus `-` sign to omit syncing the file after every logging. However, syncing will not be enabled by default.

In addition to a single `.`, we can use other operator
- ".="
- ".<="
- ".>=" (the same as `.`)
- ".<"
- ".>"

---

# Syslog - Config

The action can be file, host, user.

For example, we can use
- `/var/log/my_service.log`
- `@my_loghost` (Of course the remote host should be able to receive them)
- `@140.131.149.22`
- `root`
- `*`

---

# Syslog - `logger` Command

> `logger` - enter messages into the system logger
>
> Linux manual page

Basic Usage: `logger {message}`

Options
- `-f` - Load messages from file.
- `-p` - Priority (selector)
- `-t` - Tag (service name)

For example, when using `logger -t tag -p local0.notice "test message"`, the log will be
```systemd
Jan 11 15:45:34 <local0.notice> bsd1 tag[7621]: test message
```

<!--

`7621` is PID

-->
