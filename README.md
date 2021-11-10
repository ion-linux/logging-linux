# logging-linux

# Who provides loggign system in linux
Rsyslog
systemd-journal


# journalctl
`journalctl -u <unit>` - info about the unit
`journalctl --dmesg` - kernel messages
`journalctl -u crond --since yesterday --until 9:00 -p info` - filters combined
`journalctl UNIT=sshd` - show logs for this specific unit, tab for options
`journalctl status`

# rsyslogd
- facility - what syslogd should start logging for
- priority - 
- destination
 - file
 - user that is logged in
 - specific module

Useful when writing shell scripts (appears in logs):
> logger hello

# rsyslogd conf
/etc/rsyslog.conf
/etc/rsyslog.d/*.conf


# System Journal
rsyslogD -> /var/log
systemd-journald (via journalctl)
- not persistent, in memmory
- default written to /run/log/journal, cleared on system reboot
- if changed via Storage option in /etc/systemd/journald.conf:
  - /var/log/journal logs written here

# Who is connecting systemd-journald with rsyslogd?
systemd-journald ->>> /dev/log ->>> rsyslogd -> /var/log

> systemd-journald memory only but anyway sends logs to rsyslogd (via /var/log) which are written on disk anyways.

# Log rotation:
/etc/logrotate.conf
/etc/logrotate.d/

