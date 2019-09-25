# Lab: Rescue System From MBR Failure
The MBR could be damaged by by disk error or GRUB installation failure. This lab demosnstrate how to backup and restore MBR for boot failure.

![](fig/MBR.jpg)

## 1. Backup MBR
MBR is located in the first sector (512 bytes) of boot disk (/dev/sda).
```bash
$ dd if=/dev/sda of=mbr.bin bs=512 count=1
```
![](fig/mbr-backup.jpg)

## 2. MBR currupted and boot failed
Clean the MBR.

<B>NOTE: THIS WILL DAMAGE YOUR SYSTEM</B>
```bash
$ dd if=/dev/zero of=/dev/sda bs=446 count=1
```
The boot record is cleaned.
![](fig/mbr-currupted.jpg)

Reboot and failed.
![](fig/mbr-boot-failure.jpg)

## 3. Boot into rescue mode from CD-ROM
Use CD-ROM to boot up system to rescue mode.

- Choose "Rescue installed system"
  
  ![](fig/mbr-rescue-0.jpg)

- You don't need to enable network during rescue operations.
  
  ![](fig/mbr-rescue-1.jpg)

- Choose "Continue".
  
  ![](fig/mbr-rescue-2.jpg)

- You system will be mounted under /mnt/sysimage.
  
  ![](fig/mbr-rescue-3.jpg)
  
  ![](fig/mbr-rescue-4.jpg)

- Start the shell.
  
  ![](fig/mbr-rescue-5.jpg)

- Change root.
  ```bash
  $ chroot /mnt/sysimage
  ```

  ![](fig/mbr-chroot.jpg)

## 4. Restore MBR
Restore MBR from backup.

```bash
$ dd if=mbr.bin of=/dev/sda bs=512 count=1
```

![](fig/mbr-review.jpg)

## 5. Reboot and welcome system back