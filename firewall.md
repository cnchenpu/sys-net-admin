# Firewall
## firewalld v.s. iptables

### firewalld
* firewall-cmd
* /etc/firewalld/firewalld.conf
* /etc/firewalld/zones/
* /usr/lib/firewalld/zones
    * block.xml
    * drop.xml
    * home.xml
    * public.xml
    * work.xml
    * dmz.xml
    * external.xml
    * internal.xml
    * trusted.xml


```bash
$ firewall-cmd --state
```

### zone
id|zone|comment
---|---|---
1|public|For use in public areas. You do not trust the other computers on the network. Only selected incoming connections are accepted.
2|external|Public area, for NAT.
3|dmz|
4|work|
5|home|
6|internal|
7|trusted|

* drop zone
    - drop all incomming packets and no reply; allow outgoing connection
* block zone
    - block all incomming connection and reply 'icmp'; only allow outgoing connection

```bash
$ firewall-cmd --get-active-zone

$ firewall-cmd --get-zone

$ firewall-cmd --zone=public --list-all
```