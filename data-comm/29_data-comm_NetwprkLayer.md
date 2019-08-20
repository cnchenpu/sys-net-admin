# Netwprk Layer
![](fig/network-layer.png)

## Inter-Networking
- Hop-to-Hop

    ![](fig/hop2hop.png)

- Host-to-Host

    ![](fig/host2host.png)

## Network Layer at source
![](fig/network-layer-source.png)

## Network Layer at router
![](fig/network-layer-router.png)

## Network Layer at destination
![](fig/network-layer-destination.png)

## Switching
[ch.8](https://github.com/cnchenpu/data-comm/blob/master/17_data-comm_switch.md#switching)

- Circuit switching
    + Connection Oriented
- Packet switching
    + Virtual Circuit switching
    + Datagram switching
        + Connectionless

![](fig/datagram-switching.png)

# Addressing
- IPv4: 32 bits
- IPv6: 128 bits

## IP Address class
![](fig/IPaddress-calss.png)

![](fig/IPaddress-calss2.png)

![](fig/IPaddress-calss3.png)

## Net ID & Host ID
![](fig/netid-hostid.png)

- Network address: host ID bits all 0.
- Broadcast address: host ID bits all 1.

- Class A
    + 2<sup>7</sup>=128 networks
    + 2<sup>24</sup>=16,777,216 addresses in each network
- Class B
    + 2<sup>14</sup>=16,384 networks
    + 2<sup>16</sup>=65,536 addresses in each network
- Class C
    + 2<sup>21</sup>=2,097,152 networks
    + 2<sup>8</sup>=256 addresses in each network

## Subnet & Netmask
- Subnet: use host ID for subnet ID
- Netmask: to find the network ID

![](fig/subnet-mask.png)

## Private IP
![](fig/privateIP.png)

## NAT (Network Address Translation)
![](fig/NAT.png)

## DHCP (Dynamic Host Configuration Protocol)
![](fig/DHCP.png)


