# Linux Backup Tools

## Compress and decompress tools.
- ``zip`` / ``unzip``
- ``gzip`` / ``gunzip``
- ``zcat`` (``gunzip -c``)
- ``bzip2`` / ``bunzip2``
- ``bzcat`` - decompresses files to stdout

## dd
Convert and copy a file.
```
# dd if=/dev/sda of=/backup/root
# dd if=/dev/cdrom of=cd.iso
```

## tar
Acrhive files.
```
# tar czvf /backup-files archive.tgz
# tar xzvf archive.tgz
```

### Lab - Use 'tar' and 'find' commands to do Progressive Backup.
Progressive backup mode refers to files that have been changed or added to the storage media since the last backup.

1. Generate a change-list for file system:
   ```
   # find / -mtime -7 –print > /tmp/file-list.weekly
   ```
2. Archive files according the file change-list:
   ```
   # tar –czv –T /tmp/file-list.weekly –f weekly.tgz
   ```

## cpio
Copy files to and from archives.

- Copy-in mode: extract data.
  ```
  # cpio –idvc < back.cpio 
  ```
- Copy-out mode: create data.
  ```
  # find /backup-files | cpio –ocv> back.cpio
  ```

### Lab: Use 'cpio' to decompress boot image file: initramfs.
- In RHEL 6:
  ```
  # zcat initramfs-2.6.32-431.el6.x86_64.img | cpio -id
  ```
- In RHEL 7:
  ```
  /usr/lib/dracut/skipcpio /boot/initramfs-`uname -r`.img | gunzip -c | cpio -i -d
  ```

## scp
Secure copy. Works like 'cp' but with SSL.
```
# scp –R source-file user@ip-address:/dest-file
```

## rsync
``rsync`` works like ``scp``, but it is faster. Since ``rsync`` only copy (transfer) the file if it has changed.

- Copy file locally:
  ```
  # rsync –av /source-files /dest-path
  ```
- Push file to remote server:
  ```
  # rsync –av /source-files user@dest-IP:/dest-path
  ```
- PUll file from remote server:
  ```
  # rsync –av user@source-IP:/source-files /dest-path
  ```

Copy files twice and check the difference.
1. Copy files first time:
   ```
   # rsync -av systemd* root@172.22.14.84:/root/
   sending incremental file list
   systemd.svg
   systemd2.svg

   sent 931,507 bytes  received 57 bytes  1,863,128.00 bytes/sec
   total size is 931,138  speedup is 1.00
   ```
2. Sync the same files second time:
   ```
   # rsync -av systemd* root@172.22.14.84:/root/
   sending incremental file list

   sent 59 bytes  received 11 bytes  140.00 bytes/sec
   total size is 931,138  speedup is 13,301.97
   ```

### Lab: Copy files with 'scp' and 'rsync' multiple times, check the file's time attribute changes.
1. `scp` copy (transfer) complete files and over write their time.
2. `rsync` only copy (transfer) changes, not over write the file's time if no changes.

## ssh-keygen
Generate public key for auto login, without password.

1. Generate local RSA key pair at ``.ssh/``:
   ```
   # ssh-keygen

   # ls .ssh/
   id_rsa  id_rsa.pub  known_hosts
   ```
2. Copy the public key to the remote site:
   ```
   # scp .ssh/id_rsa.pub root@dest-IP:/root/.ssh/
   ```
3. Login to the remote site and append the public key to the ``.ssh/authorized_keys``
   ```
   # cat .ssh/id_rsa.pub >> .ssh/authorized_keys
   ```
