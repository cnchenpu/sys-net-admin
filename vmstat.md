# vmstat
***vmstat*** reports information about processes, memory, paging, block IO, traps, disks and cpu activity.

```bash
# vmstat [option] [delay [count]]

#Display active and inactive memory 
$ vmstat -a 5 3
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 3  0      0 1038160   2116 479980    0    0    66    14   59  112  1  1 98  0  0
 1  0      0 1038160   2116 479980    0    0     0     0 9885 18852  7 76 17  0  0
 1  0      0 1038160   2116 479980    0    0     0     0 9458 18020  7 74 19  0  0
```

  - Procs
       r: The number of runnable processes (running or waiting for run time).
       b: The number of processes in uninterruptible sleep.

   - Memory
       swpd: the amount of virtual memory used.
       free: the amount of idle memory.
       buff: the amount of memory used as buffers.
       cache: the amount of memory used as cache.
       inact: the amount of inactive memory.  (-a option)
       active: the amount of active memory.  (-a option)

   - Swap
       si: Amount of memory swapped in from disk (/s).
       so: Amount of memory swapped to disk (/s).

   - IO
       bi: Blocks received from a block device (blocks/s).
       bo: Blocks sent to a block device (blocks/s).

   - System
       in: The number of interrupts per second, including the clock.
       cs: The number of context switches per second.

   - CPU
       These are percentages of total CPU time.
       us: Time spent running non-kernel code. (user time, including nice time)
       sy: Time spent running kernel code. (system time)
       id: Time spent idle.
       wa: Time spent waiting for IO.
       st: Time stolen from a virtual machine.

```bash
#Report disk statistics
$ vmstat -d
disk- ------------reads------------ ------------writes----------- -----IO------
       total merged sectors      ms  total merged sectors      ms    cur    sec
sda    14034     73  851239  181643   3225   1040  184158  169861      0     78
sdb      117      0    5640      28      0      0       0       0      0      0
sr0        0      0       0       0      0      0       0       0      0      0
dm-0   13793      0  812005  180459   3928      0  153389  202179      0     78
dm-1      90      0    4920     308      0      0       0       0      0      0
```

   - Reads
       total: Total reads completed successfully
       merged: grouped reads (resulting in one I/O)
       sectors: Sectors read successfully
       ms: milliseconds spent reading

   - Writes
       total: Total writes completed successfully
       merged: grouped writes (resulting in one I/O)
       sectors: Sectors written successfully
       ms: milliseconds spent writing

   - IO
       cur: I/O in progress
       s: seconds spent for I/O

## meminfo
```bash
$ cat /proc/meminfo
MemTotal:        1882284 kB
MemFree:         1052456 kB
MemAvailable:    1336168 kB
Buffers:            2116 kB
Cached:           402684 kB
SwapCached:            0 kB
Active:           371656 kB
Inactive:         274188 kB
Active(anon):     241788 kB
Inactive(anon):     8828 kB
Active(file):     129868 kB
Inactive(file):   265360 kB
Unevictable:           0 kB
Mlocked:               0 kB
SwapTotal:       1679356 kB
SwapFree:        1679356 kB
Dirty:                 0 kB
Writeback:             0 kB
AnonPages:        241040 kB
Mapped:            92244 kB
Shmem:              9572 kB
Slab:              77244 kB
SReclaimable:      39292 kB
SUnreclaim:        37952 kB
KernelStack:        4880 kB
PageTables:        18984 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     2620496 kB
Committed_AS:    2087068 kB
VmallocTotal:   34359738367 kB
VmallocUsed:       49540 kB
VmallocChunk:   34359672824 kB
HardwareCorrupted:     0 kB
AnonHugePages:     61440 kB
CmaTotal:              0 kB
CmaFree:               0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
DirectMap4k:       90048 kB
DirectMap2M:     2007040 kB
```

