---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# Periodic Processes

siriuskoan

---

# Outline
- Introduction
- Cron
  - Config
  - `crontab` Command
- `at` Command

---

# Introduction

Sometimes we need to do something periodically.

For example,
- Send daily / weekly / monthly server report to admin
- Update system and packages every month
- Shutdown / reboot every month
- Your own shell scripts such as checking in games at 00:00.

---

# Cron

The daemon handling periodic jobs.

Config file:
- `/etc/crontab`, `/etc/cron.d/*` - System periodic jobs configuration
  - `/etc/cron.[hourly,daily,weekly,monthly]/*` - Periodic jobs
- `/etc/cron.allow` - Who can use `crontab`
- `/etc/cron.deny` - Who cannot use `crontab`

Command: `crontab`

<!--

cron.allow has higher priority than cron.deny

-->

---

# Cron - Config

`/etc/crontab` consists of many lines of command, and the command specifies when to run, run as whom, which scripts to run.

```systemd
{minute}    {hour}  {day}   {month} {day of week}   {user}  {scripts}
```

There are some special characters can be used in time format.
- `*` - Everything
- `-` - Range
- `,` - Many values
- `/` - Every time unit

<!--

`/` just like Python for loop `step` argument

-->

---

# Cron - Config

For example,

```systemd
# 10:00, from Monday to Friday
0   10  *   *   1-5
# Every 30 minute on the 15th day
0,30    *   15   *   *
# Every 2 hour
*   */2 *   *   *`
# On 10-20 and 30 minutes, from 0 to 12 o'clock
10-20,30  *   0-12    *   *
```

---

# Cron - Config

In addition to time, you can trigger cron job by event.
- `@reboot`
- `@yearly` (or `@annually`, the same as `0 0 1 1 *`)
- `@monthly` (the same as `0 0 1 * *`)
- `@weekly` (the same as `0 0 * * 0`)
- `@daily` (or `@midnight`, the same as `0 0 * * *`)
- `@hourly` (the same as `0 * * * *`)
- `@every_minute` (the same as `*/1 * * * *`)
- `@every_second`

https://nasa.cs.nctu.edu.tw/sa/2021/slides/09_Periodic_Processes.pdf (P8)

---

# Cron - `crontab` command

Users can create their own crontab by using `crontab` command.

Usage
- `crontab -l` - Show crontab
- `crontab -e` - Edit crontab

The custom crontab syntax is the same as system crontab, but user cannot specifies executor.

