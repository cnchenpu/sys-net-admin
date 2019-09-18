---
tag: sysadmin, network
---

# Debug boot failure

## Setup serial console
1. configure serial port
![](fig/serial-out.jpg)

2. add ```console=tty0 console=ttyS0,38400``` to ```/boot/grub/grub.conf``` in the end of ``kernel``

```bash
#/boot/grub/grub.conf
default=0
timeout=5
splashimage=(hd0,0)/grub/splash.xpm.gz
hiddenmenu
title CentOS 6 (2.6.32-754.el6.x86_64)
	root (hd0,0)
	kernel /vmlinuz-2.6.32-754.el6.x86_64 ro root=/dev/mapper/vg_rh6-lv_root rd_NO_LUKS LANG=en_US.UTF-8 rd_LVM_LV=vg_rh6/lv_swap rd_NO_MD SYSFONT=latarcyrheb-sun16 crashkernel=auto rd_LVM_LV=vg_rh6/lv_root  KEYBOARDTYPE=pc KEYTABLE=us rd_NO_DM rhgb quiet console=tty0 console=ttyS0,38400
	initrd /initramfs-2.6.32-754.el6.x86_64.img
```
3. check [serial.txt](./serialRH6.txt)


## Setup serial console for RHEL 7

1. Add ```console=tty0 console=ttyS0,38400 rd.debug``` to the end of ```GRUB_CMDLINE_LINUX``` in ```/etc/default/grub```
2. For UEFI, run ```grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg``` to rebuild ```grub2.conf```
3. For non-UEFI, run ```grub2-mkconfig -o /boot/grub2/grub.cfg``` to rebuild ```grub2.conf```

```bash
#/etc/default/grub
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=centos/root rd.lvm.lv=centos/swap rhgb quiet console=tty0 console=ttyS0,38400 rd.debug"
GRUB_DISABLE_RECOVERY="true"
```

- Boot messages in serial in RH7: [serial.txt](./serialRH7.txt)

