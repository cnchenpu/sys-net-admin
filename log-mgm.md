# Log Management

## journalctl
**journalctl** is a utility for querying and displaying logs from journald, systemd’s logging service.

Viewing log messages:
```bash
$ journalctl
$ journalctl -b     # boot messages
# time range
$ journalctl --since "1 hour ago"  |  --since "2 days ago"  |  --since "2019-06-26 23:00:00" --until "2019-06-26 23:20:00" 
$ journalctl -u <service-name>.service
$ journalctl -k     # show only kernel messages
$ journalctl -f     # tail -f /var/log/messages
```

## *rsyslog.service* - System Logging Service
```bash
$ systemctl list-units --type service --all | grep rsyslog
$ systemctl status rsyslog.service
```

configuration file of *rsyslogd*: /etc/rsyslog.conf
```bash
# The imjournal module bellow is now used as a message source instead of imuxsock.
$ModLoad imuxsock # provides support for local system logging (e.g. via logger command)
$ModLoad imjournal # provides access to the systemd journal
#$ModLoad imklog # reads kernel messages (the same are read from journald)
#$ModLoad immark  # provides --MARK-- message capability

# Provides TCP syslog reception
$ModLoad imtcp
$InputTCPServerRun 514

# remote host is: name/ip:port, e.g. 192.168.0.1:514, port optional
#*.* @@remote-host:514
```

### Configure centralized rsyslog server
1. Load **imtcp** module at remote-server.
2. Open **port 514** for firewall at remote-server.
   ```bash
   $ firewall-cmd --permanent --add-port=514/tcp
   $ firewall-cmd --reload
   ```
3. Add **\*.\* @@remote-host:514** in ***/etc/rsyslog.conf*** at client.
   ```bash
   $ echo "*.* @@remote-server:514" >> /etc/rsyslog.conf
   ```
4. Restart **rsyslog.service** in both hosts is necessary.

## *logrotate* - Log Rotation

configuration files: /etc/logrotate.conf
```bash
# rotate log files weekly
weekly 

# keep 4 weeks worth of backlogs
rotate 4

# create new (empty) log files after rotating old ones
create

# use date as a suffix of the rotated file
dateext

# uncomment this if you want your log files compressed
#compress

# RPM packages drop log rotation information into this directory
include /etc/logrotate.d
```

configuration files: /etc/logrotate.d/syslog
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

