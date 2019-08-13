# Firewall

![](/fig/Linux-firewall.jpg)

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
* /etc/firewalld/lockdown-whitelist.xml
* /etc/firewalld/zones/
* /etc/firewalld/services/
* /usr/lib/firewalld/services/
* /usr/lib/firewalld/zones/

### check running status:
```bash
$ firewall-cmd --state
```

### get default/active zone:
```bash
$ firewall-cmd --get-default-zone
$ firewall-cmd --get-active-zones
$ firewall-cmd --zone=public --list-all
```

#### "/etc/firewalld/zones/public.xml:"
```xml
<?xml version="1.0" encoding="utf-8"?>
<zone>
  <short>Public</short>
  <description>For use in public areas. You do not trust the other computers on networks to not harm your computer. Only selected incoming connections are accepted.</description>
  <service name="dhcpv6-client"/>
  <service name="ssh"/>
</zone>
```

### list service
```bash
$ firewall-cmd --list-services
```

#### "/usr/lib/firewalld/services/ssh.xml:"
```xml
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>SSH</short>
  <description>Secure Shell (SSH) is a protocol for logging into and executing commands on remote machines...</description>
  <port protocol="tcp" port="22"/>
</service>
```

## zone

Zones are predefined sets of rules. Network interfaces and sources can be assigned to a zone.
Predefine zone: ```/usr/lib/firewalld/zones/```

### list all zones
```bash
$ firewall-cmd --get-zones
$ firewall-cmd --list-all-zones
```

zone|comment
---|---
public|For use in public areas. You do not trust the other computers on the network. Only selected incoming connections are accepted.
external|For use on external networks with masquerading enabled, especially for routers. You do not trust the other computers on the network. Public area, for NAT.
dmz|For computers in your demilitarized zone that are publicly-accessible with limited access to your internal network.
work|For use at work where you mostly trust the other computers on the network. 
home|For use at home when you mostly trust the other computers on the network. 
internal|For use on internal networks when you mostly trust the other computers on the network.
trusted|All network connections are accepted. 
drop|Any incoming network packets are dropped without any notification. Only outgoing network connections are possible. 
block|Any incoming network connections are rejected with an icmp-host-prohibited message for IPv4 and icmp6-adm-prohibited for IPv6. Only network connections initiated from within the system are possible. 

```
Default: Only selected incoming connections are accepted.
```

### set default zone
```bash
$ firewall-cmd --set-default-zone zone-name
```

### assign (initial) zone
```bash
$ firewall-cmd --zone=home --change-interface=ens0s3
```

### use zone target to set default behavior for incoming traffic
```bash
$ firewall-cmd --zone=zone-name --list-all
$ firewall-cmd --zone=zone-name --set-target=<default|ACCEPT|REJECT|DROP>
``` 

### change fielwalld settings
```bash
$ firewall-cmd --permanent --zone=home --change-interface=ens0s3
$ firewall-cmd --reload
```

## service
Firewall services are predefined rules (```/usr/lib/firewalld/services/```) that cover all necessary settings to allow incoming traffic for a specific service and they apply within a zone. 
Services use one or more ports or addresses for network communication. Firewalls filter communication based on ports. To allow network traffic for a service, its ports must be open. firewalld blocks all traffic on ports that are not explicitly set as open. Some zones, such as trusted, allow all traffic by default.  

### get services
```bash
$ firewall-cmd --get-services
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

