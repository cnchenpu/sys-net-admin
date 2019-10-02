# GRUB (GNU GRand Unified Bootloader)
- RHEL 6: GRUB 0.97
- RHEL 7: GRUB 2
- 

## GPT disk with UEFI
```
# fdisk /dev/sda -l
WARNING: fdisk GPT support is currently new, and therefore in an experimental phase. Use at your own discretion.

Disk /dev/sda: 17.2 GB, 17179869184 bytes, 33554432 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: gpt

#         Start          End    Size  Type            Name
 1         2048       411647    200M  EFI System      EFI System Partition
 2       411648      1435647    500M  Microsoft basic 
 3      1435648     33552383   15.3G  Linux LVM
```
```
# partx /dev/sda
NR   START      END  SECTORS  SIZE NAME                 UUID
 1    2048   411647   409600  200M EFI System Partition babe9ef5-b6df-4cc1-a29b-5f1fd77d6f0b
 2  411648  1435647  1024000  500M                      5d19f200-4db2-49b2-99fa-84930b703ebe
 3 1435648 33552383 32116736 15.3G                      2bad62a6-735a-4d31-a4a4-f192427580a9
```

## Boot: UEFI
```
/boot/
├── config-3.10.0-327.el7.x86_64
├── efi
│   └── EFI
│       ├── BOOT
│       │   ├── BOOTX64.EFI
│       │   └── fallback.efi
│       └── centos
│           ├── BOOT.CSV
│           ├── fonts
│           │   └── unicode.pf2
│           ├── gcdx64.efi
│           ├── grub.cfg
│           ├── grubenv
│           ├── grubx64.efi
│           ├── MokManager.efi
│           ├── shim-centos.efi
│           └── shim.efi
├── grub
│   └── splash.xpm.gz
├── grub2
│   ├── grub.cfg
│   ├── grubenv -> /boot/efi/EFI/centos/grubenv
│   └── themes
│       └── system
├── initramfs-0-rescue-71e3e2b8e46041bb8b6387a79fd5830c.img
├── initramfs-3.10.0-327.el7.x86_64.dsf.img
├── initramfs-3.10.0-327.el7.x86_64.img
├── initramfs-3.10.0-327.el7.x86_64.img.orig
├── initramfs-3.10.0-327.el7.x86_64kdump.img
├── initramfs-dsftmp -> initramfs-3.10.0-327.el7.x86_64.img
├── initrd-plymouth.img
├── symvers-3.10.0-327.el7.x86_64.gz
├── System.map-3.10.0-327.el7.x86_64
├── vmlinuz-0-rescue-71e3e2b8e46041bb8b6387a79fd5830c
└── vmlinuz-3.10.0-327.el7.x86_64

```