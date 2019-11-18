# Systemd Timer
Timer units are useful for triggering activation of other units based on timers. You may find details in ``systemd.timer(5)``.

For example, the service of Cleanup of Temporary Directories (``systemd-tmpfiles-clean.service``) has a timer (``systemd-tmpfiles-clean.timer``) for cleaning temp directories.

The service's unit configuration file: ``/usr/lib/systemd/system/systemd-tmpfiles-clean.service``
```
[Unit]
Description=Cleanup of Temporary Directories
Documentation=man:tmpfiles.d(5) man:systemd-tmpfiles(8)
DefaultDependencies=no
Conflicts=shutdown.target
After=systemd-readahead-collect.service systemd-readahead-replay.service local-fs.target time-sync.target
Before=shutdown.target

[Service]
Type=oneshot
ExecStart=/usr/bin/systemd-tmpfiles --clean
IOSchedulingClass=idle
```

The timer's unit configuration file: ``/usr/lib/systemd/system/systemd-tmpfiles-clean.timer``
```bash
[Unit]
Description=Daily Cleanup of Temporary Directories
Documentation=man:tmpfiles.d(5) man:systemd-tmpfiles(8)

[Timer]
OnBootSec=15min         # defines a timer relative to when the machine was booted up
OnUnitActiveSec=1d      # defines a timer relative to when the unit the timer is activating was last activated.
```
This timer unit will run just 15 minutes after systemd start and every 1 day.

Time and date specifications are in the man-page ``systemd.time(7)``.

# Lab - Create service unit and a timer

## Create a service to backup root home directory

1. Create the backup directory (mkdir) ``/var/backup/root``.
   
2. Create the backup script ``/root/backup.sh``:
   ```bash
   #!/bin/bash
   DAYMONTH=`date "+%d-%H%M%S"`
   /bin/tar --selinux -czvf /var/backup/root/backup-$DAYMONTH.tgz /root &>/dev/null
   ```

3. Create the ``/usr/lib/systemd/system/backup.service`` unit file:
   ```
   [Unit]
   Description=Backup the root home directory

   [Service]
   Type=simple
   ExecStart=/root/backup.sh

   [Install]
   WantedBy=multi-user.target
   ```

4. Test to start the service (run backup).
   ```
   # systemctl daemon-reload
   # systemctl start backup.service
   ```

## Create timer for backup service

1. Create the ``/usr/lib/systemd/system/backup.timer`` unit file:
   ```
   [Unit]
   Description=Execute backup every day at midnight

   [Timer]
   OnCalendar=*-*-* 00:00:00
   Unit=backup.service

   [Install]
   WantedBy=multi-user.target
   ```

2. Enable and start the timer.
   ```
   # systemctl enable backup.timer
   # systemctl is-enabled backup.timer
   # systemctl start backup.timer
   # systemctl is-active backup.timer
   ```

3. Check the timer.
   ```
   # systemctl list-timers --all | grep backup
   ```
