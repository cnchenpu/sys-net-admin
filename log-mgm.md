# Log Management

Log file location: ``/var/log/``.
```
-rw-------. 1 root   root    57065 Nov 18 13:01 messages
-rw-------. 1 root   root   295806 Oct 27 03:29 messages-20191027
-rw-------. 1 root   root   312702 Nov  3 03:14 messages-20191103
-rw-------. 1 root   root   446771 Nov 10 03:46 messages-20191110
-rw-------. 1 root   root   551505 Nov 17 03:10 messages-20191117
```

## systemd-journald
**systemd-journald** is a system service (```systemd-journald.service```) that collects and stores logging data. The configuration file is ```/etc/systemd/journald.conf```.

## journalctl
**journalctl** is a utility for querying and displaying logs from systemd’s journal (```systemd-journald.service```). 

### Viewing journal log messages:
```bash
$ journalctl

$ journalctl --list-boots     # a list of pervious boots
$ journalctl -b     # boot messages
$ journalctl -k     # show only kernel messages
$ journalctl -f     # view the log in reat time, tail -f /var/log/messages
$ journalctl -n 10     # show the last 10 events

# time range
$ journalctl --since "1 hour ago"   
$ journalctl --since "2 days ago"
$ journalctl --since today
$ journalctl --since yesterday --until now
$ journalctl --since "2019-06-26 23:00:00" --until "2019-06-26 23:20:00" 

# filter by units
$ journalctl -u <service-name>.service      # show only specific service log
$ journalctl -u sshd.service
$ journalctl -u sshd.service -x     # -x: show more detail
$ journalctl -u sshd.service --since yesterday     # show sshd's log since yesterday

# filter by error level
# error level: "emerg" (0), "alert" (1), "crit" (2), "err" (3), "warning" (4), "notice" (5), "info" (6), "debug" (7)
$ journalctl -p err
```

## Log Rotation

The configure file: ``/etc/logrotate.conf``.
```bash
# rotate log files size=20M
size=20M

# rotate log files weekly
weekly

# keep 4 weeks worth of backlogs
rotate 4

# create new (empty) log files after rotating old ones
create

# use date as a suffix of the rotated file
dateext

# uncomment this if you want your log files compressed
compress

# RPM packages drop log rotation information into this directory
include /etc/logrotate.d
```

Configuration files for log rotation: ``/etc/logrotate.d/syslog``
```bash
/var/log/cron
/var/log/maillog
/var/log/messages
/var/log/secure
/var/log/spooler
{
    # If the log file is missing, go on to the next one without issuing an error message.
    missingok

    # If sharedscripts is specified, the scripts are only run once.
    sharedscripts

    # The command will be executed after the log file is rotated.
    postrotate
        # HUP - Lets rsyslogd close all open files.
        /bin/kill -HUP `cat /var/run/syslogd.pid 2> /dev/null` 2> /dev/null || true
    endscript
}
```

## Log flow
Journal events can be transferred to traditional syslog daemon (***syslogd***). The syslog daemon behaves like a normal journal client, and reads messages from the journal files, similarly to journalctl.

![](fig/log-flow.jpg)

## *rsyslog.service* - System Logging Service (extened of traditional *syslogd*)
```bash
$ systemctl list-units --type service --all | grep rsyslog
$ systemctl status rsyslog.service
```
