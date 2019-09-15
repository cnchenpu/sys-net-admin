# Lab: Packet Sniffing with Wireshark
Use Wireshark to learn how to do packet sniffing.

Download: <a href="https://www.wireshark.org/download.html">![Wireshark](https://www.wireshark.org/assets/theme-2015/images/wireshark_logo@2x.png)</a>

An illustration of capture an incomming frame: <br>
![](fig/packet-sniffing.png)


## ipconfig & arp
![](fig/ipconfig.png)

![](data-comm/fig/../../fig/arp-a.png)

Start the Wireshark: <br>
![](fig/wireshark-start.png)

## ARP 
ARP packet format: <br>
![](fig/ARP-packet.jpg) 

ARP Request: <br>
![](fig/ARP-request.png)

ARP Reply: <br>
![](fig/ARP-reply.png)

# HW: Use Wireshare to analyze the ARP protocol
- Start your web browser and clear the browser’s cache memory, but do not access any website yet.
- Open the Wireshark and start capturing.
- Go to your browser and retrieve any file from a website. Wireshark starts capturing packets.
- In the filter field of the Wireshark window type arp (lower case) and click Apply. Stop capturing and save the captured file.

Answer these questions:
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
  
