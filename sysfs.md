# sysfs - /sys/ virtual file system 
Hardware information has been moved from ***procfs*** to ***sysfs***.

- **/sys/block** - for each block device in the system.
- **/sys/bus** - for system bus.
- **/sys/class** - function type of device: input, network, and block devices.
- **/sys/devices/platform** - for peripheral devices.
- **/sys/devices/system** - for non-peripheral devices.
- **/sys/firmware** - for firmware objects.
- **/sys/module** - for all loaded kernel moduels.
- **/sys/power** - contorl system power state.

# Runtime kernel parameters - /proc/sys/
- Check runtime kernel parameters: ```sysctl -a``` command.
- Change parameter: edit ```/etc/sysctl.conf``` or ```echo``` value to the parameter file.

