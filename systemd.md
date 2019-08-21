# systemd
The *systemd*, system and service manager, is responsible for controlling how services are started, stopped and managed. It is backward compatible with *init scripts* used by previous versions of Linux.

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

### Check service configurations:
```bash
$ cat /usr/lib/systemd/system/<service name>.service

$ systemctl cat <service-name>.service
```

### Edit service unit (configuration) file:
```bash
$ systemctl edit --full <service-name>.service
```

### E.q.: firewalld.service unit file (```systemctl cat firewalld.service``` or ```/usr/lib/systemd/system/firewalld.service```):
```bash
# /etc/systemd/system/firewalld.service
[Unit]
Description=firewalld - dynamic firewall daemon
Before=network-pre.target   # Before: this (firewalld) service will be started BEFORE the 'network-pre.target'
Wants=network-pre.target    # Wants: the 'network-pre.target' will be started when this (firewalld) service starts. The firewalld WANTS the network-pre.target, but not necessary.
After=dbus.service          # After: this (firewalld) service will be started AFTER the 'dbus.service'
After=polkit.service
Conflicts=iptables.service ip6tables.service ebtables.service ipset.service
Documentation=man:firewalld(1)

[Service]
EnvironmentFile=-/etc/sysconfig/firewalld
ExecStart=/usr/sbin/firewalld --nofork --nopid $FIREWALLD_ARGS
ExecReload=/bin/kill -HUP $MAINPID
# supress to log debug and error output also to /var/log/messages
StandardOutput=null
StandardError=null
Type=dbus
BusName=org.fedoraproject.FirewallD1
KillMode=mixed

[Install]
WantedBy=multi-user.target  # WantedBy: the firewalls is WANTEDBY the 'multi-user.target', but not necessary. The firewalld will be started after 'multi-user.target' starts.
Alias=dbus-org.fedoraproject.FirewallD1.service
```


### Display the status of all services:
```bash
#service utility
$ service --status-all
#chkconfig utility
$ chkconfig --list 
$ chkconfig --list <service-name>
#systemctl utility
$ systemctl list-units --type service --all
$ systemctl list-unit-files --type service
```

### Starting and stopping services:
```bash
#service utility
$ service <service-name> status | start | stop | restart 
#systemctl utility
$ systemctl status | start | stop | restart | reload <service-name>
```

### Enabling and disabling services:
```bash
#chkconfig utility
$ chkconfig <service-name> on | off
#systemctl utility
$ systemctl enable | disable <service-name>
```

### Show service detail info:
```bash
$ systemctl show <service-name>
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

### List the currently active targets:
```bash
$ systemctl list-units --type target
```

### Check default target mode:
```bash
$ systemctl get-default
$ ll /etc/systemd/system/default.target
$ runlevel
```

### Set default target mode:
```bash
$ systemctl set-default multi-user.target
```

### Changing system states:
```bash
$ systemctl reboot      #reboot (reboot.target)
$ systemctl poweroff    #power off (poweroff.target)
$ systemctl emergency   #put in emergency mode (emergency.target)
$ systemctl default     #back to default mode (multi-user.target)
```

### Viewing log messages:
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