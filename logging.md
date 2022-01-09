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
  - `logger` command

---

# Why Do We Need Log Files?

- We can know what went wrong when an incident happened
- We can know who did what before an incident happened

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

