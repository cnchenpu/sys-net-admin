# QUIZ: Open port for client's iperf connections in server.

## Create service definition file /usr/lib/firewalld/services/iperf3.xml:
```xml
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>IPERF3</short>
  <description>iperf3 is a network performance testing tool.</description>
  <port protocol="tcp" port="5201"/>
  <port protocol="udp" port="5201"/>
</service>
```

## Reload firewalld and check new service
```bash
$ firewall-cmd --reload
$ firewall-cmd --info-service=iperf3
```

## Check firewall status and current firewalld zone:
```bash
$ firewall-cmd --state
$ firewall-cmd --get-active-zones
$ firewall-cmd --zone=public --list-all
```

## Add service iperf3 to current (public) zone:
```bash
$ firewall-cmd --add-service=iperf3 --zone=public

$ firewall-cmd --zone=public --list-all
```
