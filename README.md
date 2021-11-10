# logging-linux

# Who provides loggign system in linux
Rsyslog
systemd-journal


# journalctl
`journalctl -u <unit>` - info about the unit\
`journalctl --dmesg` - kernel messages\
`journalctl -u crond --since yesterday --until 9:00 -p info` - filters combined\
`journalctl UNIT=sshd` - show logs for this specific unit, tab for options\
`journalctl status`\

# rsyslogd
- facility - what syslogd should start logging for
- priority - 
- destination
 - file
 - user that is logged in
 - specific module

Useful when writing shell scripts (appears in logs):
> logger -s -p user.info TESTING HELLO

# rsyslogd conf
/etc/rsyslog.conf
/etc/rsyslog.d/*.conf
> writes logs to /var/log\

# System Journal
systemd-journald (via journalctl)
- not persistent, in memmory
- default written to /run/log/journal, cleared on system reboot
- if changed via Storage option in /etc/systemd/journald.conf:
  - /var/log/journal logs written here

# Who is connecting systemd-journald with rsyslogd?
systemd-journald ->>> /dev/log ->>> rsyslogd -> /var/log

> systemd-journald memory only but anyway sends logs to rsyslogd (via /var/log) which are written on disk anyways.

# Log rotation:
/etc/logrotate.conf\
/etc/logrotate.d/


# Configure rsyslog to receive logs from remote location
Sending machine
```
# for TCP use @@IP
cron.* @@1.2.3.4:514

# for UDP use @IP
ftp.* @6.7.8.9:514
```

Receiving
```
# Notloaded by default
# Provides UDP syslog reception
$ModLoad imudp
$UDPServerRun 514

# Provides TCP syslog reception
$ModLoad imtcp
$InputTCPServerRun 514

# Log all facilities, all levels at this template
$template RemoteLogs, "/var/log/%HOSTNAME%/%PROGRAMNAME%.log"
*.* ?RemoteLogs
& ~
# above line says once logs are received then will be processed and writeen to file
```

Verify logs are received:
```
tcpdump host IP-HOSTNAME
```
```
ss -tulpan | grep syslog
```






