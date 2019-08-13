# systemd
The *systemd*, system and service manager, is responsible for controlling how services are started, stopped and managed. It is backward compatible with *init scripts* used by previous versions of Linux.

## systemd v.s. init scripts

## unit

```/usr/lib/systemd/system```

unit | function | extention
---|---|---
service unit | Start and control daemons and the processes they consist of. | .service
target unit | Replaces init run levels. | .target
mount unit | Contorl mount points in the file system. | .mount
device unit | Expose kernel devices in systemd | .device
snapshot unit | Can be used to temporarily save the state of the set of systemd units, which can later be restored by activating the saved snapshot unit. | .snapshot
swap unit | Encapsulate memory swap partitions or swap files. | .swap

## target