---
tags: sysadmin, Linux
---

# Lab: Upgrading to GRUB 2     

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

### 4. Replace GRUB legacy bootloader by reinstall GRUB 2
   - On BIOS-based machines:
     ```
     $ grub2-install /dev/sda
     ```
   - On UEFI-based machines:
     ```
     $ yum reinstall grub2-efi shim
     ```

