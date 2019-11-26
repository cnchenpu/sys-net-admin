---
tags: sysadmin, Linux
---

# Linux Firewall - firewalld

A firewall is a way to protect machines from any unwanted traffic from outside. It enables users to control incoming network traffic on host machines by defining a set of **firewall rules**. These rules are used to sort the incoming traffic and either **block** it or **allow** through. 

## firewalld v.s. iptables

The essential differences between ***firewalld*** and the ***iptables*** (and ip6tables) services are:
- The **iptables** service stores configuration in ```/etc/sysconfig/iptables``` and ```/etc/sysconfig/ip6tables```, while **firewalld** stores it in various XML files in ```/usr/lib/firewalld/``` and ```/etc/firewalld/```. Note that the ```/etc/sysconfig/iptables``` file does not exist as **firewalld** is installed by default on RHEL 7.x.
- With the **iptables** service, every single change means flushing all the old rules and reading all the new rules from ```/etc/sysconfig/iptables```, while with **firewalld** there is no recreating of all the rules. Only the differences are applied. Consequently, **firewalld** can change the settings during runtime without existing connections being lost. Unlike the iptables command, the ``firewall-cmd`` command does not restart the firewall and disrupt established TCP connections.

To use the **iptables** services instead of **firewalld**, needs to disable **firewalld** service and install ***iptables-services*** package.
1. Disable and stop firewalld:
  ```
  - systemctl disable firewalld
  - systemctl stop firewalld
  ```
2. Install iptables-services package:
  ```
  - yum install iptables-services
  ```
3. Start and enable iptables:
  ```
  - systemctl start iptables
  - systemctl enable iptables
  ```
4. Configure iptables rules.  

## firewalld
***firewalld*** uses the concepts of ***zones*** and ***services***, that simplify the traffic management. **Zones** are predefined sets of rules. Network interfaces and sources can be assigned to a zone. The traffic allowed depends on the network your computer is connected to and the security level this network is assigned. Firewall **services** are predefined rules that cover all necessary settings to allow incoming traffic for a specific service and they apply within a zone. 

### enable/start/disable firewalld
```bash
$ systemctl enable firewalld
$ systemctl start firewalld
$ systemctl restart firewalld
$ systemctl disable firewalld
```

### firewalld zone and services configuration files:

* /etc/firewalld/zones/
* /etc/firewalld/services/
* /usr/lib/firewalld/services/
* /usr/lib/firewalld/zones/

### check running status:
```bash
$ firewall-cmd --state
$ systemctl status firewalld
```

### get default/active zone:
```bash
$ firewall-cmd --get-default-zone
$ firewall-cmd --get-active-zones
$ firewall-cmd --zone=public --list-all
```

```=0
$ firewall-cmd --zone=public --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens192
  sources: 
  services: ssh dhcpv6-client
  ports: 
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
```


#### "/etc/firewalld/zones/public.xml:"
```xml=
<?xml version="1.0" encoding="utf-8"?>
<zone>
  <short>Public</short>
  <description>For use in public areas. You do not trust the other computers on networks to not harm your computer. Only selected incoming connections are accepted.</description>
  <service name="dhcpv6-client"/>
  <service name="ssh"/>
</zone>
```

### list open services
```bash
$ firewall-cmd --list-services
```

### list open ports
```bash
$ firewall-cmd --list-ports
```

#### "/usr/lib/firewalld/services/ssh.xml:"
```xml=
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
$ firewall-cmd --get-default-zone
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


### set default zone and add network interface to the zone (if not exist)
- default: Only selected incoming connections are accepted.

```bash
$ firewall-cmd --set-default-zone <zone-name>
$ firewall-cmd --zone=<zone-name> --add-interface=eth0
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
Firewall services are predefined rules that cover all necessary settings to allow incoming traffic for a specific service and they apply within a zone. The configuration files for the default supported services are located at ```/usr/lib/firewalld/services``` and user-created service files would be in ```/etc/firewalld/services```.
Services use one or more ports or addresses for network communication. Firewalls filter communication based on ports. To allow network traffic for a service, its ports must be open. firewalld blocks all traffic on ports that are not explicitly set as open. Some zones, such as trusted, allow all traffic by default.  

### get all services
```bash
# /usr/lib/firewalld/services/
$ firewall-cmd --get-services
```

### get a specific service info
```bash
$ firewall-cmd --info-service=<service-name>
```
```=0
$firewall-cmd --info-service=ssh
ssh
  ports: 22/tcp
  protocols: 
  source-ports: 
  modules: 
  destination: 
```

### add service to zone
```bash
$ firewall-cmd --add-service=ssh --zone=public
```

### add port to zone
```bash
$ firewall-cmd --zone=public --add-port=9958/tcp
```

### remove service from zone
```bash
$ firewall-cmd --zone=public --remove-service=http --permanent
```

### create new service (add port) by command
```bash=
# new service
$ firewall-cmd --permanent --new-service=iperf3

# add description for new service
$ firewall-cmd --permanent --service=iperf3 --set-description="iperf3 is an network performance testing tool"

# add short description of the service
$ firewall-cmd --permanent --service=iperf3 --set-short=iperf3

# add service ports
$ firewall-cmd --permanent --service=iperf3 --add-port=5201/tcp
$ firewall-cmd --permanent --service=iperf3 --add-port=5201/udp

# reload firewalld configurations
$ firewall-cmd --reload

# check new service
$ firewall-cmd --get-services | grep iperf3
$ firewall-cmd --info-service=iperf3
```

### create new service (add port) by xml file
1. create service xml file at ```/usr/lib/firewalld/services/service-name.xml```

```xml=
<!-- /usr/lib/firewalld/services/iperf3.xml -->
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>iperf3</short>
  <description>iperf3 is a network performance testing tool.</description>
  <port protocol="tcp" port="5201"/>
  <port protocol="udp" port="5201"/>
</service>
```

2. reload firewalld configurations (service xml file have changed)
```bash
$ firewall-cmd --reload
```

3. add the service to zone
```bash
$ firewall-cmd --add-service=iperf3 --zone=public
```

### disable firewalld
```bash
$ systemctl disable firewalld
```

### disable/enabe all traffic
```bash
$ firewall-cmd --panic-on
$ firewall-cmd --panic-off
$ firewall-cmd --query-panic
```

## Advanced firewalld configuration with Rich Language

The rich language extends the current zone elements. With the rich language more complex firewall rules can be created in an easy to understand way. For more detail, check ```man firewalld.richlanguage```.

E.q.:
1. Allow new IPv4 connections from address 192.168.0.0/24 for service tftp.
  
  ```$ firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" source address="192.168.0.0/24" service name="tftp"' ```

2. Deny IPv4 traffic over TCP from host 192.168.1.10 to port 22.

   ```$ firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" source address="192.168.1.10" port port=22 protocol=tcp reject'``` 

3. White-list source address to allow all connections from 192.168.2.2.

   ```$ firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" source address="192.168.2.2" accept'```

4. Black-list source address to reject all connections from 192.168.2.3.

   ```$ firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" source address="192.168.2.3" reject type="icmp-admin-prohibited"'```

5. Black-list source address to drop all connections from 192.168.2.4

   ```$ firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" source address="192.168.2.4" drop'```

## Lab: Open connection port for iperf3 service

The **iperf3** is a network testing tool. User initiate an **iperf3** service on a server and run another **iperf3** on a client, then **iperf3** can test the connection throughput between the client and server. You can check [here](https://hackmd.io/SHVMza8-T1WqOVkZmNt1bA#iperf) for the command usage.

The **iperf3** server listen to the **5201** port, so firewall need to open the port **5201** for client/server connection. Use RHEL 7 for server and RHEL 6 for client.

- Hint: 
    1. You have to create an **iperf3** service definition XML file for **firewalld**.
    2. Then add the service to the default **zone**.