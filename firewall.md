# Firewall
A firewall is a way to protect machines from any unwanted traffic from outside. It enables users to control incoming network traffic on host machines by defining a set of firewall rules. These rules are used to sort the incoming traffic and either block it or allow through. 

## firewalld v.s. iptables

## firewalld
***firewalld*** uses the concepts of ***zones*** and ***services***, that simplify the traffic management. **Zones** are predefined sets of rules. Network interfaces and sources can be assigned to a zone. The traffic allowed depends on the network your computer is connected to and the security level this network is assigned. Firewall **services** are predefined rules that cover all necessary settings to allow incoming traffic for a specific service and they apply within a zone. 

### firewalld
```bash
$ systemctl status firewalld
$ systemctl enable firewalld
$ systemctl start firewalld
```

* firewall-cmd
* /etc/firewalld/firewalld.conf
* /etc/firewalld/zones/
* /etc/firewalld/services/
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

Predefine zone: ```/usr/lib/firewalld/zones/```


id|zone|comment
---|---|---
1|public|For use in public areas. You do not trust the other computers on the network. Only selected incoming connections are accepted.
2|external|For use on external networks with masquerading enabled, especially for routers. You do not trust the other computers on the network. Public area, for NAT.
3|dmz|For computers in your demilitarized zone that are publicly-accessible with limited access to your internal network.
4|work|For use at work where you mostly trust the other computers on the network. 
5|home|For use at home when you mostly trust the other computers on the network. 
6|internal|For use on internal networks when you mostly trust the other computers on the network.
7|trusted|All network connections are accepted. 
-|drop|Any incoming network packets are dropped without any notification. Only outgoing network connections are possible. 
-|block|Any incoming network connections are rejected with an icmp-host-prohibited message for IPv4 and icmp6-adm-prohibited for IPv6. Only network connections initiated from within the system are possible. 

```
default: Only selected incoming connections are accepted.
```

### list zone status
```bash
$ firewall-cmd --get-active-zone
$ firewall-cmd --get-zone
$ firewall-cmd --list-all-zones
$ firewall-cmd --zone=public --list-all
```

### get/set default zone
```bash
$ firewall-cmd --get-default-zone
$ firewall-cmd --set-default-zone zone-name
```

### use zone target to set default behavior for incoming traffic
```bash
$ firewall-cmd --zone=zone-name --list-all
$ firewall-cmd --zone=zone-name --set-target=<default|ACCEPT|REJECT|DROP>
``` 

### assign (initial) zone
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
$ firewall-cmd --list-services
```

### add service to zone
```bash
$ firewall-cmd --zone=public --add-services=dns
$ firewall-cmd --add-service=ssh --zone=public
```

### add port to zone
```bash
$ firewall-cmd --zone=public --add-port=9958/tcp
```

### disable firewalld
```bash
$ systemctl disable firewalld
```

### disable/enabe all traffic
```bash
firewall-cmd --panic-on
firewall-cmd --panic-off
firewall-cmd --query-panic
```

