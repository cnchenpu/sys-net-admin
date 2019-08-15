# systemd
The *systemd*, system and service manager, is responsible for controlling how services are started, stopped and managed. It is backward compatible with *init scripts* used by previous versions of Linux.

View systemd information:
```bash
$

## unit
A **unit** file ```/usr/lib/systemd/system/<resource name>.<unit type>``` basically describes a resource and tells **systemd** how to activate that resource.

unit type | function | extention
---|---|---
service unit | Start and control daemons and the processes they consist of. | .service
target unit | Replaces init run levels. | .target
mount unit | Contorl mount points in the file system. | .mount
device unit | Expose kernel devices in systemd | .device
snapshot unit | Can be used to temporarily save the state of the set of systemd units, which can later be restored by activating the saved snapshot unit. | .snapshot
swap unit | Encapsulate memory swap partitions or swap files. | .swap

## systemd service units v.s. init scripts

Previous versions of Linux use *init scripts* located in the ```/etc/rc.d/init.d/``` directory to start and stop services. In RHEL 7, these init scripts have been replaced with *systemd service units*. 

## service

Display the status of all services:
```bash
#service utility
$ service --status-all
#chkconfig utility
$ chkconfig --list 
$ chkconfig --list <name>
#systemctl utility
$ systemctl list-units --type service --all
$ systemctl list-unit-files --type service
```

Starting and stopping services:
```bash
#service utility
$ service <name> status | start | stop | restart 
#systemctl utility
$ systemctl status | start | stop | restart <name>
```

Enabling and disabling services:
```bash
#chkconfig utility
$ chkconfig <name> on | off
#systemctl utility
$ systemctl enable | disable <name>
```

Show service info:
```bash
$ systemctl show <name>
```

## target
A ***target unit*** is a special kind of unit file because it doesn’t represent a single resource; rather, it groups other units to bring the system to a particular state. Target units in systemd loosely resemble run levels in System V in the sense that each target unit represents a particular system state.
In RHEL 7, *run levels* have been replaced with *systemd target units*. Target units have a **.target** extension and similar to run levels. 

```/usr/lib/systemd/system/runlevel*.target```

run level | target units | description
---|---|---
0 | runlevel0.target, poweroff.target | shutdown and power off
1 | runlevel1.target, rescue.target | rescue shell for maintenance mode
2, 3, 4 | runlevel[234].target, multi-user.target | non-graphical multi-user mode
5 | runlevel5.target, graphical.target | graphical multi-user mode
6 | runlevel6.target, reboot.target | system reboot

List the currently active targets:
```bash
$ systemctl list-units --type target
```

Check default target mode:
```bash
$ systemctl get-default
$ ll /etc/systemd/system/default.target
$ runlevel
```

Set default target mode:
```bash
$ systemctl set-default multi-user.target
```

Changing system states:
```bash
$ systemctl reboot      #reboot (reboot.target)
$ systemctl poweroff    #power off (poweroff.target)
$ systemctl emergency   #put in emergency mode (emergency.target)
$ systemctl default     #back to default mode (multi-user.target)
```

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

### Use *systemd-analyze* to check server's boot problem
```bash
$ systemd-analyze
$ systemd-analyze time 
$ systemd-analyze blame
$ systemd-analyze critical-chain
$ systemd-analyze plot > systemd.svg
$ systemd-analyze dot | dot -Tsvg > systemd2.svg
```
![](fig/systemd.svg)
![](fig/systemd2.svg)