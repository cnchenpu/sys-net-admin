# Control Group - Cgroup

**cgroups** (control groups) is a Linux kernel feature that limits, accounts for, and isolates the resource usage (CPU, memory, disk I/O, network, etc.) of a collection of processes. 

**cgroups** provides:
- **Resource limiting** - groups can be set to not exceed a configured memory limit, which also includes the file system cache.
- **Prioritization** - some groups may get a larger share of CPU utilization or disk I/O throughput.
- **Accounting** - measures a group's resource usage, which may be used, for example, for billing purposes.
- **Control** - freezing groups of processes, their checkpointing and restarting.

Available controller:
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