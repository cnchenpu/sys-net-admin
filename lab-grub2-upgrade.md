---
tags: sysadmin, Linux
---

# Lab: Upgrading to GRUB 2     
When you do an in-place upgrade of Red Hat Enterprise Linux (RHEL) from version 6 to 7, the upgrade from GRUB Legacy to GRUB 2 does not happen automatically, but it should be done manually.

### 1. Remove GRUB legacy package:
```
$ yum remove grub
``` 

### 2. Install GRUB 2 package:
```
$ yum install grub2
```

### 3. Generating the GRUB 2 configuration files:
    
1. Install the GRUB 2 files to the ``/boot/grub/`` directory of system disk (/dev/sda):
    ```
    $ grub2-install --grub-setup=/bin/true /dev/sda
    ```  

2. Generate the grub.cfg: 
   - On BIOS-based machines:
        ```
        $ grub2-mkconfig -o /boot/grub2/grub.cfg
        ```
    - On UEFI-based machines:
        ```
        $ grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg
        ```
    
    ```
    Note the difference in the configuration file extensions:
      .conf is for GRUB
      .cfg is for GRUB 2 
    ```    

### 4. Testing GRUB 2 with GRUB Legacy bootloader still installed
To safely test GRUB 2 configuration, we will start GRUB 2 from GRUB Legacy.

1. Add a new section into ``/boot/grub/grub.conf``: 
   - The ``/boot`` partition is in the ``/dev/sda1``.
   ```
    title GRUB 2 Test
	    root (hd0,0)
	    kernel /grub2/i386-pc/core.img
	    boot
   ```
2. Reboot the system. 
3. Select the ``GRUB 2`` Test entry when the GRUB menu presented.
4. When presented with a ``GRUB 2`` menu, select a kernel to boot.
5. If the above did not work, restart, and do not choose the ``GRUB 2`` Test entry on next boot.  

### 5. Replace GRUB legacy bootloader by reinstall GRUB 2
   - On BIOS-based machines:
     ```
     $ grub2-install /dev/sda
     ```
   - On UEFI-based machines:
     ```
     $ yum reinstall grub2-efi shim
     ```

# HW4 (due date: 10/22):
- Upgrade your RHEL 6 VM to RHEL 7 version and than upgrade the boot loader from GRUB legacy to GRUB 2.
- Take a VM snapshot before you do this lab.
- Demo the result to me in the class.