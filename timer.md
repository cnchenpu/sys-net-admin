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
The timer unit will run just 15 minutes after systemd start and every 1 day.

Time and date specifications are in the man-page ``systemd.time(7)``.

