# Life of a Packet

- Process a packet goes through in a network
- Same topology in 11 notes
- using PC1 -> R1 -> R2 -> R4 -> PC4
- each interface on the router has a MAC address
- excluding switches from analysis
  - the switches learn the ethernet addresses after each ARP request and reply
  - both for DST and SRC devices and updates its table
  - it does not de-encapsulate the packets
  - it always forwards the packet without tampering with it
- the routers do the de-encapsulation, update the ethernet frame and then forward the packet
  - the SRC and DST MAC addresses will change throughout the process
  - the IP addresses will never change


## MAC addresses and IP addresses

- PC1 -> 1111
- R1 
  - G0/2 interface -> AAAA
  - G0/0 interface -> BBBB
- R2
  - G0/0 interface -> CCCC
  - G0/1 interface -> DDDD
- R4
  - G0/1 interface -> EEEE
  - G0/2 interface -> FFFE
    - not FF:FF because of broadcast
- PC4 -> 4444

- PC1 - Src IP address: 192.168.1.1
- PC4 - Dst IP address: 192.168.4.1


## Process

### PC1 to R1

- PC1 notices PC4 is not in its network
- So it has to send the packet to its default gateway
- it uses ARP to get default gateway MAC address 
  - ARP request(the order matters here) - BROADCAST
    - Src IP -> 192.168.1.1 -> PC1 IP
    - Dst IP -> 192.168.1.254  -> Router IP
    - Dst MAC -> FFFF:FFFF:FFFF -> broadcast
    - Src MAC -> 1111 -> PC1 Mac
    - Switch 1 is connected to PC1 and R1
      - it broadcasts the message, gets the MAC of R1 and replies to PC1
  - ARP reply - UNICAST
    - Src IP -> 192.168.1.254 -> Router IP
    - Dst IP -> 192.168.1.1 -> PC1 IP
    - Dst MAC -> AAAA
    - Src MAC -> 1111

- New packet from PC1 to R1 -> encapsulate, and change MAC of packet
  - Src IP: 192.168.1.1
  - Dst IP: 192.168.4.1
  - Dst MAC: AAAA -> R1 interface connected to PC1
  - Src MAC: 1111 -> PC1 interface connected to R1


### R1 to R2

- R1 receives the packet, removes the layer 2 MAC address and checks its routing table for the next hop for the Dst IP
  - it sees the next hop as 192.168.12.2 (R2)
  - it creates an ARP request to get the MAC address of R2
    - Src IP: 192.168.12.1
    - Dst IP: 192.168.12.2
    - Dst MAC: FFFF -> broadcast to get MAC address of Dst IP
    - Src MAC: BBBB -> exit interface G0/0 from R1 to R2
  - R2 sees this and replies with its MAC address at G0/0
    - Src IP: 192.168.12.2
    - Dst IP: 192.168.12.1
    - Dst MAC: BBBB
    - Src MAC: CCCC -> interface connected to R1 from R2

- new packet from R1 to R2 -> change MAC
  - Src IP: 192.168.1.1
  - Dst IP: 192.168.4.1
  - Dst MAC: CCCC -> R2 interface connected to R1
  - Src MAC: BBBB -> R1 interface connected to R2


### R2 to R4

- R2 receives the packet, removes the layer 2 MAC address and checks its routing table for the next hop for the Dst IP
  - it sees the next hop as 192.168.24.4 (R4)
  - it creates an ARP request to get the MAC address of R4
    - Src IP: 192.168.24.2
    - Dst IP: 192.168.24.4
    - Dst MAC: FFFF -> broadcast to get MAC address of Dst IP
    - Src MAC: DDDD -> exit interface G0/1 from R2 to R4
  - R4 sees this and replies with its MAC address at G0/1
    - Src IP: 192.168.24.4
    - Dst IP: 192.168.24.2
    - Dst MAC: DDDD
    - Src MAC: EEEE -> interface connected to R2 from R4

- new packet from R2 to R4 -> change MAC
  - Src IP: 192.168.1.1
  - Dst IP: 192.168.4.1
  - Dst MAC: EEEE -> R4 interface connected to R2
  - Src MAC: DDDD -> R2 interface connected to R4


### R4 to PC4

- R4 receives the packet, removes the layer 2 MAC address and checks its routing table for the next hop for the Dst IP
  - There is no next hop, it is directly connected here
  - it creates an ARP request to get the MAC address of PC4
    - Src IP: 192.168.4.254
    - Dst IP: 192.168.4.4
    - Dst MAC: FFFF -> broadcast to get MAC address of Dst IP
    - Src MAC: FFFE -> exit interface G0/1 from R2 to R4
  - R4 sees this and replies with its MAC address at G0/1
    - Src IP: 192.168.4.4
    - Dst IP: 192.168.4.254
    - Dst MAC: FFFE
    - Src MAC: 4444 -> interface connected to R2 from R4

- new packet from R4 to PC4 -> change MAC
  - Src IP: 192.168.1.1
  - Dst IP: 192.168.4.1
  - Dst MAC: 4444 -> PC4 interface connected to R4
  - Src MAC: FFFE -> R4 interface connected to PC4


### Reply from PC4 to PC1

- there will be no ARP requests
  - the MAC addresses are already on the devices, so it is just forwarded
