# Net-SNMP


## check snmpd's running port
```bash
# use netstat to get port and process info (a:all, u:udp, t:tcp, n:number, p:program) 
$ netstat -autnp | grep snmp
tcp        0      0 127.0.0.1:199           0.0.0.0:*               LISTEN      24835/snmpd         
udp        0      0 0.0.0.0:161             0.0.0.0:*                           24835/snmpd
```

## open snmp ports for firewalld
1. create service file: ```/usr/lib/firewalld/services/snmpd.xml```
```xml
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>snmpd</short>
  <description>snmpd for network performance monitoring.</description>
  <port protocol="tcp" port="199"/>
  <port protocol="udp" port="161"/>
</service>
```

2. add the service to zone
```bash
$ firewall-cmd --zone=public --add-services=snmpd
```

3. reload firewalld configurations
```bash
$ firewall-cmd --reload
```

