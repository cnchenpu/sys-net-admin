---
tags: sysadmin, networking
---

# Lab: Packet Sniffing with Wireshark
Use Wireshark to learn how to do packet sniffing.

Download: <a href="https://www.wireshark.org/download.html">![Wireshark](https://www.wireshark.org/assets/theme-2015/images/wireshark_logo@2x.png)</a>

An illustration of capture an incomming frame: <br>
![fig/packet-sniffing.png](https://i.imgur.com/YRpKtqi.png)


## ipconfig & arp
![fig/ipconfig.png](https://i.imgur.com/BGfHUtS.png)

![fig/arp-a.png](https://i.imgur.com/xN4eL67.png)


Start the Wireshark: <br>
![fig/wireshark-start.png](https://i.imgur.com/Efhxnpy.png)


## ARP 
ARP packet format: <br>
![fig/ARP-packet.jpg](https://i.imgur.com/cZxGUyu.jpg)


ARP Request: <br>
![fig/ARP-request.png](https://i.imgur.com/ubTvZao.png)


ARP Reply: <br>
![fig/ARP-reply.png](https://i.imgur.com/NoTTmNU.png)

## 3 Way Handshake
![](https://upload.wikimedia.org/wikipedia/commons/3/3f/Connection_TCP.png)

![wireshark-ex1-3way-handshake.jpg](https://i.imgur.com/ctc5RG6.jpg)

- [Example](https://github.com/cnchenpu/sys-net-admin/blob/master/wireshark-ex1-3way-handshake.pcapng?raw=true) of 3 Way Handshake.


## 4 Way Handshake
![](https://upload.wikimedia.org/wikipedia/commons/thumb/5/55/TCP_CLOSE.svg/1024px-TCP_CLOSE.svg.png)


## HTTP
- plant txt
- ![](fig/wireshark-ex4-http.jpg)

- connection initiation (3 way handshake)
  ![](fig/wireshark-ex4-http-conn-init.jpg)

- connection closed (4 way handshake)
  ![](fig/wireshark-ex4-http-conn-end.jpg)

- [Example](https://github.com/cnchenpu/sys-net-admin/blob/master/wireshark-ex2-http.pcapng?raw=true) of HTTP.


## DNS query & response
![dns query](https://i.imgur.com/dzoZqw1.png)

![dns response](https://i.imgur.com/ONjQNRn.jpg)


## TLS Handshake
![TLS protocol](https://i.imgur.com/6IvoUg6.png)

![wireshark-ex3-TLS-client-hello.png](https://i.imgur.com/ks0IM23.png)

![wireshark-ex3-TLS-server-hello.png](https://i.imgur.com/zVGWbQY.png)



# HW: Use Wireshare to analyze the ARP protocol (Due date: 10/24)
- Start your web browser and clear the browser’s cache memory, but do not access any website yet.
- Open the Wireshark and start capturing.
- Go to your browser and retrieve any file from a website. Wireshark starts capturing packets.
- In the filter field of the Wireshark window type arp (lower case) and click Apply. Stop capturing and save the captured file.

#### Put you homework in HackMD, then email me the link to ```cnchen @ pu.edu.tw```, email subject format: ```[HW1_you-student-ID]```

#### Answer these questions:
- Part I: ARP request message
  - a. the hardware type.
  - b. the protocol type.
  - c. the hardware length.
  - d. the protocol length.
  - e. the operation code.
  - f. the source hardware address.
  - g. the source protocol address?
  - h. the destination hardware address.
  - i. the destination protocol address?
  
- Part II: ARP reply message
  - a. the hardware type.
  - b. the protocol type.
  - c. the hardware length.
  - d. the protocol length.
  - e. the operation code.
  - f. the source hardware address.
  - g. the source protocol address?
  - h. the destination hardware address.
  - i. the destination protocol address?
  
