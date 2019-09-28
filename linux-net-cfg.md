# Linux Network Configuration

## Configuration file
```bash
# cat /etc/sysconfig/network-scripts/ifcfg-eth0 
DEVICE=eth0
TYPE=Ethernet
UUID=abcb8fbe-fac3-4af5-a5c9-dc03746c3f2a
ONBOOT=yes
NM_CONTROLLED=yes
#BOOTPROTO=dhcp
BOOTPROTO=static
IPADDR=172.22.2.222
PREFIX=23
GATEWAY=172.22.2.1
DNS=8.8.8.8
##
DEFROUTE=yes
IPV4_FAILURE_FATAL=yes
IPV6INIT=no
NAME="System eth0"
HWADDR=08:00:27:37:6B:5B
PEERDNS=yes
PEERROUTES=yes
LAST_CONNECT=1568257891
```

## Configure commands
### ifconfig
```
# ifconfig
eth0      Link encap:Ethernet  HWaddr 08:00:27:45:58:D3  
          inet addr:192.168.0.183  Bcast:192.168.0.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe45:58d3/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:3905 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2183 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:374848 (366.0 KiB)  TX bytes:231915 (226.4 KiB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:0 (0.0 b)  TX bytes:0 (0.0 b)
```

### ip addr
```
# ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 08:00:27:45:58:d3 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.183/24 brd 192.168.0.255 scope global eth0
    inet 10.0.0.1/24 scope global eth0
    inet6 fe80::a00:27ff:fe45:58d3/64 scope link 
       valid_lft forever preferred_lft forever
```
``` 
# ip addr add 10.0.0.1/24 dev eth0
```
```
# ip addr del 10.0.0.1/24 dev eth0
```

```bash
# nmcli connection show
NAME    UUID                                  TYPE      DEVICE 
enp0s3  0dbe726b-1064-45b2-a9e1-063982887115  ethernet  enp0s3 

# nmcli device show
GENERAL.DEVICE:                         enp0s3
GENERAL.TYPE:                           ethernet
GENERAL.HWADDR:                         08:00:27:3A:9D:65
GENERAL.MTU:                            1500
GENERAL.STATE:                          100 (connected)
GENERAL.CONNECTION:                     enp0s3
GENERAL.CON-PATH:                       /org/freedesktop/NetworkManager/ActiveConnection/1
WIRED-PROPERTIES.CARRIER:               on
IP4.ADDRESS[1]:                         172.22.2.13/23
IP4.GATEWAY:                            172.22.2.1
IP4.ROUTE[1]:                           dst = 0.0.0.0/0, nh = 172.22.2.1, mt = 100
IP4.ROUTE[2]:                           dst = 172.22.2.0/23, nh = 0.0.0.0, mt = 100
IP4.DNS[1]:                             172.22.6.236
IP4.DNS[2]:                             10.197.10.253
IP6.ADDRESS[1]:                         fe80::47f9:a76b:c1f7:48df/64
IP6.GATEWAY:                            --
IP6.ROUTE[1]:                           dst = fe80::/64, nh = ::, mt = 100
IP6.ROUTE[2]:                           dst = ff00::/8, nh = ::, mt = 256, table=255

GENERAL.DEVICE:                         lo
GENERAL.TYPE:                           loopback
GENERAL.HWADDR:                         00:00:00:00:00:00
GENERAL.MTU:                            65536
GENERAL.STATE:                          10 (unmanaged)
GENERAL.CONNECTION:                     --
GENERAL.CON-PATH:                       --
IP4.ADDRESS[1]:                         127.0.0.1/8
IP4.GATEWAY:                            --
IP6.ADDRESS[1]:                         ::1/128
IP6.GATEWAY:                            --
```

```bash
# hostnamectl 
   Static hostname: RH7
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 1f50519909994dfbb26e03d0e599fd05
           Boot ID: eef9ad2c37504720b78417838c459f75
    Virtualization: kvm
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-957.el7.x86_64
      Architecture: x86-64
```

