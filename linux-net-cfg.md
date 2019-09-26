# Linux Network Configuration

## RHEL 6 network configuration
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

## RHEL 7 network configuration
```bash
# cat /etc/sysconfig/network-scripts/ifcfg-enp0s3 
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="dhcp"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="enp0s3"
UUID="0dbe726b-1064-45b2-a9e1-063982887115"
DEVICE="enp0s3"
ONBOOT="yes"
```

```bash
# ifconfig
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.22.2.13  netmask 255.255.254.0  broadcast 172.22.3.255
        inet6 fe80::47f9:a76b:c1f7:48df  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:3a:9d:65  txqueuelen 1000  (Ethernet)
        RX packets 2216  bytes 209684 (204.7 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 580  bytes 256709 (250.6 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 48  bytes 4944 (4.8 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 48  bytes 4944 (4.8 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
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

