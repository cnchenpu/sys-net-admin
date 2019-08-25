# Control Group - Cgroup - Resource Management

**cgroups** (control groups) is a Linux kernel feature which allow processes to be organized into hierarchical groups whose usage of various types of resources (CPU, memory, disk I/O, network,...) can then be limited and monitored. 

![](fig/cgroups.jpg)

**cgroups** provides:
- **Resource limiting** - groups can be set to not exceed a configured memory limit, which also includes the file system cache.
- **Prioritization** - some groups may get a larger share of CPU utilization or disk I/O throughput.
- **Accounting** - measures a group's resource usage, which may be used, for example, for billing purposes.
- **Control** - freezing groups of processes, their checkpointing and restarting.

Available subsystem (resource controller):
```bash
$ more /proc/cgroups 
#subsys_name	hierarchy	  num_cgroups    enabled
cpuset	                2	            1	       1
cpu	                    3	            1	       1
cpuacct	                3	            1	       1
memory	                9	            1	       1
devices	                7	           24	       1
freezer	               10	            1	       1
net_cls	                5	            1	       1
blkio	                4	            1	       1
perf_event	            8	            1	       1
hugetlb	                6	            1	       1
pids	               11	            1	       1
net_prio	            5	            1	       1
```

- **cpuset** - assigns individual CPUs and memory nodes to tasks in a cgroup.
- **cpu** - uses the CPU scheduler to provide cgroup tasks access to the CPU.
- **cpuset** - creates automatic reports on CPU resources used by tasks in a cgroup.
- **memory** - sets limits on memory use by tasks in a cgroup.
- **devices** - allows or denies access to devices for tasks in a cgroup.
- **freezer** - suspends or resumes tasks in a cgroup.
- **net_cls** - tags network packets that allows traffic control.
- **blkio** - sets limits on input/output access to and from block devices.
- **perf_event** - enables monitoring cgroups with the perf tool.
- **hugetlb** - allows to use virtual memory pages of large sizes and to enforce resource limits on these pages.
- **pids** - limits the total process for a cgroup.
- **net_prio** - sets network interface priority.

**systemd** manage **cgroups**. **systemd** automatically creates a hierarchy of ***slice***, ***scope*** and ***service*** units to provide a unified structure for the cgroup tree. Use ```systemd-cgls``` command to check **cgroup tree**.

![](fig/cgroups-2.jpg)

- **user.slice** — the default place for all user sessions.
- **system.slice** - the default place for all system services.

```systemd-cgtop``` command shows **cgroups** resource usage.

## Example of resource allocation
1. Create systemd unit file for CPU stress tests.
```bash
# cat /etc/systemd/system/stress1.service
[Unit]
Description=Put some stress

[Service]
Type=Simple
ExecStart=/usr/bin/dd if=/dev/zero of=/dev/null
####
# cat /etc/systemd/system/stress2.service
[Unit]
Description=Put some stress

[Service]
Type=Simple
ExecStart=/usr/bin/dd if=/dev/zero of=/dev/null
```

2. Start these services (**system slice**) and check CPU usage.
```bash
$ systemctl daemon-reload
$ systemctl start stress1
$ systemctl start stress1

$ top
  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1994 root      20   0  107992    608    516 R 49.7  0.0   0:03.11 dd
 2001 root      20   0  107992    612    516 R 49.7  0.0   0:02.21 dd
 # Two processes get CPU resource equally
```

3. Run another backgroup process (**user slice**), then check CPU usage.
```bash
$ while true; do true; done &

$ top
  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1983 root      20   0  116220   1404    152 R 32.9  0.0   1:53.28 bash
 2193 root      20   0  107992    608    516 R 32.9  0.0   0:07.59 dd
 2200 root      20   0  107992    612    516 R 32.9  0.0   0:07.13 dd
# Three processes get CPU resource equally
```

4. Enable **DefaultCPUAccounting**, **DefaultBlockIOAccounting** and **DefaultMemoryAccounting** in the ```/etc/systemd/system.conf``` then reboot.

5. Redo above tests and check CPU usage.
```bash
$ systemctl start stress1
$ systemctl start stress2
$ while true; do true; done &

$ top
  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 2132 root      20   0  116220   1520    392 R 49.3  0.0   1:16.47 bash
 1994 root      20   0  107992    608    516 R 24.8  0.0   2:30.40 dd
 2001 root      20   0  107992    612    516 R 24.8  0.0   2:29.50 dd
# The user slice is now able to claim 50% of the CPU while the system slice is divided at ~25% for both the stress service.
``` 

6. Limits CPU usages (**CPUShares**) for the stress services by editing the unit files.
```bash
# cat /etc/systemd/system/stress1.service
[Unit]
Description=Put some stress

[Service]
CPUShares=512
Type=Simple
ExecStart=/usr/bin/dd if=/dev/zero of=/dev/null
####
# cat /etc/systemd/system/stress2.service
[Unit]
Description=Put some stress

[Service]
CPUShares=1024
Type=Simple
ExecStart=/usr/bin/dd if=/dev/zero of=/dev/null
# allow stress2 double to stress1 of the resource allocation  
```

7. Reload systemd and stress services
```bash
$ systemctl daemon-reload
$ systemctl restart stress1
$ systemctl restart stress2
```

8. Check CPU usage.
```bash
$ top
  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 2132 root      20   0  116220   1520    392 R 49.7  0.0   2:43.11 bash
 2414 root      20   0  107992    612    516 R 33.1  0.0   0:04.85 dd
 2421 root      20   0  107992    608    516 R 16.6  0.0   0:01.95 dd
# out of the available 50% CPU resources for system.slice, stress2 gets double the CPU allocated to stress1 service.
```
