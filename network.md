# Computer Network

What we need to care about the computer network?
- connect people
- extend services
- scale
- availibility
- security
So, we need to know how it work and how to manage it.
- network structure
- connection media
- access link
- switching and routing
- protocol
- network services
- performance: delay, data loss and throughput
- security: flaw, encryption and authentication

## Network Topology
![](fig/topology.png)

## Connection Media
- UTP (Unshielded Twisted Pair)
- STP (Shielded Twisted Pain)
- Coaxial Cable
- Optical Fiber
- Radio Wave

## Scaling (Network Equipment)
- MODEM (MOdulation-DEModulation)
  - Cable MODEM
  - ADSL MODEM
- NIC (Network Interface Card)
- Repeater
  - Regenerates the original bits
  - for physical layer
- Hub
  - Multi-port Repeater
  - Broadcast
  - for physical layer
- Bridge
  - Connecting two network island
  - Connect two segments by forwarding boradcast MAC address
  - for data-link layer
- Switched Hub
  - Multi-port Bridge
  - Layer 3 Switch
  - Virtual LAN
  - for data-link layer
  
  ![](fig/hub-vs-switch.png)

- Router
  -  A router works by looking at the IP address in the data packet and decides if it is for internal use or if the packet should move outside the network (to the WAN).
  - for network layer
- Gateway
  - A gateway acts as a conversion from one protocol to another.
  - for application layer

![](data-comm/fig/connection-devices.png)

## Protocol
Different languages for different layers.

[Data Communications in Networks.](https://hackmd.io/rXs0HZ2qR2CWWV4peYQ4eg?view)

[Data-Link Layer](https://hackmd.io/z3zmxkqxSQy8wzcuP1vrqQ?view)

[IEEE 802 LAN Architecture](https://hackmd.io/SC8X7r-TR7-6keImQijgMA?edit)

[Lab: Packet Sniffing with Wireshark](https://hackmd.io/VmrgongvR9Os1Li1-XfdDA?view)