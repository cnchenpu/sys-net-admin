# Net-SNMP


## check snmpd's running port
```bash
# use netstat to get port and process info (a:all, u:udp, t:tcp, n:number, p:program) 
$ netstat -autnp | grep snmp
tcp        0      0 127.0.0.1:199           0.0.0.0:*               LISTEN      24835/snmpd         
udp        0      0 0.0.0.0:161             0.0.0.0:*                           24835/snmpd
```

## open snmp ports (udp:161) for firewalld
```bash
#1. check snmp service info
$ firewall-cmd --info-service=snmp

#2. add the snmp service (port 161) to zone for firewalld
$ firewall-cmd --add-service=snmp --zone=public

#3. reload firewalld configurations
$ firewall-cmd --reload

#4. check firewalld zone or services
$ firewall-cmd --list-all
$ firewall-cmd --list-services
```

