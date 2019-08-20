Please refer to textbook [chapter 13](https://github.com/cnchenpu/data-comm/blob/master/ppt/Ch13-Forouzan.ppt) and [chapter 17](https://github.com/cnchenpu/data-comm/blob/master/ppt/Ch17-Forouzan.ppt).

This week we are going to discuss the majority LAN protocol - Ethernet. 
Before that, let's take a look about general networking concerns.

# General Network Issues (Pro and Con)
- Selecting the transmission medium
  - Wired
  - Wireless
- Selecting the link configuration
  - Point-to-point
    - dedicated link for users high, predictable performance
    - need many links, planning of end-points
  - Point-to-multipoint
    - cover multiple users with single link
    - requires sharing of medium: Media Access Control protocol for multiple access
      -  allow one station to use the shared medium at a time 
- Coverage area
  - LAN
  - WAN
  - Backbone
  
  ![](fig/backbone-network.png)
  
- Network topology
  - Mesh
    - each pair of nodes have dedicated point-to-point link
    - complex and expansive for large network
    - addresses not needed in frames
  - Bus
    - early Ethernet 
    - single multipoint link connects all stations
    - require protocols to share medium
    - faulty link stops all communications
    - limited number of stations
  - Ring
    - Token Ring
    - same as Bus topology
  - Star
    - today most LAN use Star topology
    - fault tolerance for links

![](fig/network-topology.png)

# IEEE 802 LAN Architecture
- Physical (PHY) Layer
- Data Link (DL) Layer
  - Logical Link Control (LLC)
    - addressing, connecting to other networks
  - Media Access Contorl (MAC)
    - efficient data transfer, sharing the medium
- EX: IEEE 802.3 (Ethernet), IEEE 802.5 (Token Ring), IEEE 802.11 (WiFi), IEEE 802.15.1 (Bluetooth), IEEE 802.16 (WiMax),... 

![](fig/IEEE-Ethernet.png)

# Ethernet (IEEE 802.3) 10BaseT
- Most popular packet-switched LAN technology
- Bandwidths: 10Mbps, 100Mbps, 1Gbps, 10Gbps
- Max bus length: 2500m 
  - 500m segments with 4 repeaters
- [1-persistent](https://github.com/cnchenpu/data-comm/blob/master/24_data-comm_datalink-MAC.md#how-to-sense-the-media-is-busy-or-available) [CSMA/CD](https://github.com/cnchenpu/data-comm/blob/master/24_data-comm_datalink-MAC.md#carrire-sense-multiple-access-with-collision-detection-csmacd)
  - listens to line before/during sending
  - If line is idle (no carrier sensed)
    - send packet immediately
    - wait 96 bits time (__inter packet gap__) between frames - 9.6 &mu;s for 10M bps Ethernet.
  - If line is busy (carrier sensed)
    - wait until idle and transmit packet immediately (1-persistent)
  - If collision detected
    - Stop sending and jam signal
    - Try again later

## IEEE 802.3 MAC frame format
![](fig/802.3-MAC-frame.png)

[Let's take a look the Ethernet frame format from Wireshark outputs.](https://github.com/cnchenpu/data-comm/blob/master/20_data-comm_lab1-wireshark-arp.md)

- A valid Ethernet frame size is from  64  to  1518  bytes.
  - the shortest frame size is determined by the Etherent's __Collision Detection__ needs
  - the longer frame size the bigger buffer (memory) needs
- Preamble: 56 bits (7 bytes) of alternating 1s and 0s 
  - define in physical layer
- Start Frame Delimiter (SFD): 10101011 (1 byte)
- Destination Address (DA): MAC address of receiver 
  - 6 bytes
- Source Address (SA): MAC address of self (sender) 
  - 6 bytes
- Type: Identify the protocol type of upper layer, EX: ARP.
  - 2 bytes
- Data: Data from upper layer
  - minimum payload length: 46 bytes
  - maximum palyoad length: 1500 bytes
  - divide data into multiple frames if data length larger than the limitation
  - The largest possible payload in a frame is called the Maximum Transmission Unit __(MTU)__
    - Jumbo Frame
- CRC: CRC check - 4 bytes.

## Why Ethernet frames must be at least 64 bytes long?
- Because the need of Collision Detection.
  - The maximum distance of Ethernet is 2500m between hosts.
    - Assume the speed of electron is 1x10<sup>8</sup> m/sec (speed of light is 3x10<sup>8</sup> m/sec).
    - So 2500m needs 25 &mu;s, we use 25.6 &mu;s.
  - The frame must be alive in 2 times (a round-trip) of longest transmission time for collision detection.
    - So a round-trip time (RTT) needs 2x25.6 = 51.2 &mu;s.
  - For 10Mbps Ethernet, it need 0.1 &mu;s for 1 bit, so 51.2 &mu;s can send 512 bits, that is 64 bytes.
  - Fast Ethernet has the same 64 byte minimum frame size but can get bits out 10 times faster.
    - RTT: 5.12 &mu;s
    - Shorter the network range.
  
![](fig/ethernet-collision.png)

## Jumbo Frame
- Larger MTUs allow greater efficiency in data transmission since each frame carries more user data (payload) while protocol overhead and underlying per-packet delay remain fixed. 
- The effective use of Jumbo frames requires that every link along the network path support the same Jumbo frame MTU.
- Application:
  - Server Clustering  
  - Server Backups (larger MTUs permit faster backups) 
  - High Speed Supercomputer Interconnect (for data transfer, not messaging) 
  - Network File Server (NFS) Protocol (9000 byte MTU to carry an 8192 NFS data block) 
  - iSCSI SANs (9000 bytes to reduce the effect of TCP frame overhead) 
  - FCoE SANs (2500 bytes to enclose an FC frame of 2000 bytes) 
- Sending data in Jumbo frames results in fewer frames being sent across the network. 
- Processing fewer frames generates conservation of CPU cycles and thus greater throughput.

![](fig/jumbo-frame-perf.png)

- Larger frames consume more Ethernet link transmission time, causing greater delays for those packets that follow and thus increasing latency.      
- This could have a negative consequence for applications that require low latency and consist of smaller packet sizes such as Voice over IP.

![](fig/frame-trans-time.png)

# MAC address
![](fig/mac-address.png)

- Ethernet (48 bits, 6 bytes) 
- Unicase address
  - A3:34:45:11:92:F1
- Multicase address
  - A2:34:45:11:92:F1
- Broadcase address
  - FF:FF:FF:FF:FF:FF

# Ethernet Problem
Ethernet throughput worst with:
- More hosts
  - More collisions needed to identify single sender
- Smaller packet sizes
  - More frequent arbitration
- Longer links
  - Collisions take longer to observe, more wasted bandwidth
- Efficiency is improved by avoiding these conditions

# Evolution of Ethernet
![](fig/Ethernet-evolution.png)

- Bandwidth Enhancement
  - comptible: not change frame format
  - higher speed, shorter transmission range
- Hub/Reapter
  - layer 1 intermediate system (acts on bits)
  - enlarge the signal power to extend the network range
  - the same collision domain
- Bridge Ethernet (Layer 2 Switch)
  - connect 2 segments (the same protocol)
  - layer 2 intermediate system
  - forward layer 2 broadcast
  - seprate collision domains
  - use MAC address for switching
  - keep the mapping of MAC addresses and switch ports
  - use broadcast to find the MAC address if it not found in table
  - Bridging: software based
  - LAN switching: hardware (ASIC) based

![](fig/bridge-ethernet.png)

![](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e8/Network_Bridging.png/800px-Network_Bridging.png)
  
- Layer 3 Switch (IP Switch, Switch Router)
  - keep routing table  
  - use IP address for switching
  - limit the broadcast packets
- Virtual LAN
  - 802.1q
  - use VLAN tags for switching

![](fig/switched-ethernet.png)

- Full-Duplex Switched Ethernet
  - send and receive data at the same time
  - each station is connected to the switch via two seprate links
  - each link is a point-to-point dedicated path between the station and the switch
  - CSMA/CD is not needed
  
![](fig/full-duplex-switch-ethernet.png)

# Fast Ethernet (IEEE 802.3u) 100BaseTx
- Fast Ethernet uses the same frame formats and CSMA/CD technology as normal 10Mbps Ethernet. 
- Network distance: 100m.
- The difference is that the maximum delay for the signal across the segment is now 5.12 &mu;s instead of 51.2 &mu;s.
  - [Why Ethernet frames must be at least 64 bytes long?](https://github.com/cnchenpu/data-comm/blob/master/25_data-comm_LAN.md#why-ethernet-frames-must-be-at-least-64-bytes-long)
- The Inter-Packet Gap (IPG) for 802.3u is 0.96 &mu;s as opposed to 9.6 &mu;s for 10Mbps Ethernet. 

# Gigabits Ethernet (IEEE 802.3ab) 1000BaseT
- Network distance: 100m.
- The inter-packet gap is 96 ns.

## For a given minimum-length frame, the extent of a network scales inversely with data rate.
![](fig/ethernet-extents.png)

# Virtual LAN (VLAN)
- a switched network that is logically segmented
- a broadcast domain that exists within a defined set of switches

![](fig/switch-vlan.png)
