# tcpdump

The ***tcpdump*** utility allows you to capture packets that flow within your network to assist in network troubleshooting.

options | description
--- | ---
-D | Display list of network interfaces
-i | Specify an interface to capture
-c | Specify the count of packets to receive
-v, -vv, -vvv | Increase the level of detail (verbos mode)
-w | Write captured data to file
-r | Read captured data from file

Examples of tcpdump utility:
```bash
#Display traffic between 2 hosts
$ tcpdump host <host1> and <host2>

#Display traffic from a source or a destination host
$ tcpdump src <host>
$ tcpdump dst <host>

#Display traffic for a specific protocol
$ tcpdump tcp

#Filtering based on source or destination port
$ tcpdump src port ftp
$ tcpdump dst port http
```
