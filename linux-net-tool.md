# Linux Network Analysis Tools

## netstat
```
# netstat -ntup
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address         Foreign Address         State         PID/Program name   
tcp        0      0 192.168.0.183:22      192.168.0.176:64914     ESTABLISHED   2245/sshd   
tcp        0     64 192.168.0.183:22      192.168.0.176:50109     ESTABLISHED   5804/sshd
```
```
# netstat -r
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt  Iface
192.168.0.0     *               255.255.255.0   U         0 0          0  eth0
default         192.168.0.1     0.0.0.0         UG        0 0          0  eth0
```

## ss (socket statistics)
```
# ss
State       Recv-Q Send-Q        Local Address:Port         Peer Address:Port   
ESTAB       0      0             192.168.0.183:ssh          192.168.0.176:64914   
ESTAB       0      0             192.168.0.183:ssh          192.168.0.176:50109
```

## arping
To detect duplicate IP
```
# arping -D 192.168.0.163 
ARPING 192.168.0.163 from 0.0.0.0 eth0
Unicast reply from 192.168.0.163 [08:00:27:13:63:5B]  1.141ms
Sent 1 probes (1 broadcast(s))
Received 1 response(s)
```

## ip neigh
```
# ip neigh
192.168.0.163 dev eth0 lladdr 08:00:27:13:63:5b STALE
192.168.0.1   dev eth0 lladdr 68:ff:7b:56:06:8e STALE
192.168.0.176 dev eth0 lladdr 9c:4e:36:9c:40:d8 DELAY
```

## nmap
```
# nmap -sn 192.168.0.0/24
...
Nmap scan report for 192.168.0.189
Host is up (0.014s latency).
MAC Address: 00:AE:FA:FD:75:01 (Unknown)
Nmap scan report for 192.168.0.240
Host is up (0.00022s latency).
MAC Address: 20:CF:30:B5:F5:BB (Asustek Computer)
Nmap done: 256 IP addresses (9 hosts up) scanned in 3.11 seconds
```

## nc (ncat)
![](fig/ncat.jpg)

```nc``` (```ncat```) reads and writes data across the network from the command line.

- Listen for connections on TCP port 8080.
  
  ```nc -l 8080```

- Send a file over TCP port 9899 from host2 (client) to host1 (server).
  
  ```HOST1$ nc -l 9899 > outputfile```

  ```HOST2$ nc HOST1 9899 < inputfile```

- Transfer in the other direction, turning nc into a "one file" server.

  ```HOST1$ nc -l 9899 < inputfile```

  ```HOST2$ nc HOST1 9899 > outputfile```

- Enable communication between host1 (client) and host2 (server).
 
  ```HOST1$ nc -l 8000```

  ```HOST2$ nc HOST1 8000```

- Port scanning.
  
  ```$ nc -z host.example.com 20-30```

  