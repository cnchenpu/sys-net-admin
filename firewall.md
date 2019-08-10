# Firewall
A firewall is a way to protect machines from any unwanted traffic from outside. It enables users to control incoming network traffic on host machines by defining a set of firewall rules. These rules are used to sort the incoming traffic and either block it or allow through. 

## firewalld v.s. iptables

### firewalld
* firewall-cmd
* /etc/firewalld/firewalld.conf
* /etc/firewalld/zones/
* /usr/lib/firewalld/services/
* /usr/lib/firewalld/zones/
    * block.xml
    * drop.xml
    * home.xml
    * public.xml
    * work.xml
    * dmz.xml
    * external.xml
    * internal.xml
    * trusted.xml

### check running status:
```bash
$ firewall-cmd --state
```

## zone
Zones are predefined sets of rules. Network interfaces and sources can be assigned to a zone.

id|zone|comment
---|---|---
1|public|For use in public areas. You do not trust the other computers on the network. Only selected incoming connections are accepted.
2|external|Public area, for NAT.
3|dmz|
4|work|
5|home|
6|internal|
7|trusted|
-|drop|drop all incomming packets and no reply; allow outgoing connection
-|block|block all incomming connection and reply 'icmp'; only allow outgoing connection

### list zone status
```bash
$ firewall-cmd --get-active-zone
$ firewall-cmd --get-zone
$ firewall-cmd --zone=public --list-all
```

### initial zone
```bash
$ firewall-cmd --zone=home --change-interface=ens0s3
```
### change fielwalld settings
```bash
$ firewall-cmd --permanent --zone=home --change-interface=ens0s3
$ firewall-cmd --reload
```

## service
Firewall services are predefined rules that cover all necessary settings to allow incoming traffic for a specific service and they apply within a zone. 
Services use one or more ports or addresses for network communication. Firewalls filter communication based on ports. To allow network traffic for a service, its ports must be open. firewalld blocks all traffic on ports that are not explicitly set as open. Some zones, such as trusted, allow all traffic by default.  

### list service & port
```bash
$ firewall-cmd --get-services
```

### add service to zone
```bash
$ firewall-cmd --zone=public --add-services=dns
```

### add port to zone
```bash
$ firewall-cmd --zone=public --add-port=9958/tcp
```

### disable firewalld
```bash
$ systemctl disable firewalld
```
