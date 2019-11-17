---
tags: sysadmin, Linux
---

# Logical Volume Management
![](fig/lvm.png)

### The configuration file: /etc/lvm/lvm.conf
```
# By default we accept every block device:
    filter = [ "a/.*/" ]
# Exclude the cdrom drive
# filter = [ "r|/dev/cdrom|" ]
```

### The Device-Mapper device
![](fig/lvm-dm.png)

### PV basic commands: pvcreate, pvs, pvscan, pvdisplay
![](fig/lvm-pv.png)

### VG basic commands: vgcreate, vgs, vgscan, vgdisplay
![](fig/lvm-vg.png)

### LV basic commands: lvcreate, lvs, lvscan, lvdisplay
![](fig/lvm-lv.png)

## LVM Limitation
Depends on the PE (physical extent) definition.
- Maxium PE numbers: (2^16 = 65536)
- Default PE size: 4 MB
  
  ![](fig/lvm-pe.png)

Create 8M PE: ```vgcreate -s 8m vg1 /dev/sdd1```

## LVM Extend
1. pvcreate
   
      ```pvcreate /dev/sde2```

2. vgextend
   
    ```vgextend vg1 /dev/sde2```

3. lvextend {-l|--extents | -L|--size  [+] …}
   
    ```lvextend +100M /dev/vg1/lvol0```

4. resize2fs
   
    ```resize2fs /dev/vg1/lvol0```

## LVM Shrink
1. unmount
2. e2fsck
3. resize2fs
   
   ```resize2fs /dev/vg1/lvol0 100M```

4. lvreduce {-l|--extents | -L|--size  [-] …} 
   
   ```lvreduce -L 100M /dev/vg1/lvol0```

5. vgreduce
   
   ```vgreduce vg1 /dev/sde2```

6. pvremove
   
   ```pvremove /dev/sde2```

