# Log Management

## *rsyslog.service* - System Logging Service
```bash
$ systemctl list-units --type service --all | grep rsyslog
$ systemctl status rsyslog.service
```

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
        # SIGHUP - Reloads the complete daemon configuration.
        /bin/kill -HUP `cat /var/run/syslogd.pid 2> /dev/null` 2> /dev/null || true
    endscript
}
```

