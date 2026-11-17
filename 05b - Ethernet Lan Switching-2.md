# Ethernet LAN Switching 2

- Preamble + SFD is usually not considered part of the Ethernet header
  - but is usually sent with it
- The size of the Ethernet header + trailer is 18 bytes
  - without the preamle and SFD
- minimum size for an ethernet frame
  - Header + payload[packet] + trailer is 64 bytes
  - which means the minimum payload is 46 bytes 
- if the payload is less than 46 bytes, padding bytes are added
  - the value of the padding is 00

- GNS3 
  - run real Cisco IoS
  - similar to Packet Tracer
- G0/0 - Gigabit ethernet interface
- Aging
  - when a switch removes the mac addresses from its table 
    - after about 5 minutes of inactivity


## ARP

- Address Resolution Protocol
- it is used to discover the Layer 2 address(MAC) of a known Layer 3 address(IP address))
- it consists of 2 messages
  - ARP request
    - sent by the device that wants to know the address of the other device
  - ARP reply
    - sent to the requesting device, containing information of the MAC address
- in the example with the setup below,
  - PC1 would send ARP request
  - PC3 would send ARP reply
- ARP request is sent as a broadcast
  - sent to all hosts on the network
- ARP reply is unicast
  - sent to one host(the host that sent the request)


## ARP Table

- a table linking MAC addresses to IP addresses in a device
- it is usually updated after sending an ARP request and receiving an ARP reply


## Setup

- Switch 1 - SW1
  - G0/0 - PC1 - 0C:2F:B0:11:9D:00
    - 192.168.1.1
  - G0/1 - PC2 - 0C:24:B0:84:62:00
    - 192.168.1.2
  - G0/2 - SW2
- Switch 2 - SW2
  - G0/0 - PC3 - 0C:2F:B0:6A:39:00
    - 192.168.1.3
  - G0/1 - PC4 - 0C:2F:B0:1E:0A:00
    - 192.168.1.4
  - G0/2 - SW1
- IP address
  - 192.168.1.0/24 - Network address
- IP packet for sending from PC1 to PC3
  - Source IP: 192.168.1.1
  - Destination IP: 192.168.1.3
  - Source MAC: ...11:9D:00
  - Destination MAC: ???
    - unknown because it doesn't know it at first

- ARP request sent before the Ethernet frame with the packet
  - Source IP: 192.168.1.1
  - Destination IP: 192.168.1.3
  - Source MAC: 0C:2F:80:11:9D:00
  - Destination MAC: FF:FF:FF:FF:FF:FF
    - broadcast to all devices
    - broadcast mac address
- ARP reply sent after resolution from PC3
  - Source IP: 192.168.1.3
  - Destination IP: 192.168.1.1
  - Source MAC: 0C:2F:B0:6A:39:00
  - Destination MAC: 0C:2F:80:11:9D:00
    - unicast 

### ARP sending

- PC1 trying to find out the MAC address of PC3
- it sends an ARP request to the switch with the details above
  - the switch saves the MAC address of the source in its MAC address table
    - 0C:2F:80:11:9D:00 on interface G0/0
  - it notices the destination is a broadcast MAC address: FF:FF:FF...
    - it then broadcasts the request to every interface except the source
    - G0/1 - PC2 - ignores it, because the IP address does not match it
    - G0/2 - SW2 - switch 2 receiving the ARP request
      - sees the ARP request
      - then stores the source in its MAC address table
        - 0C:2F:80:11:9D:00 on interface G0/2
      - it notices the broadcast MAC address and the forwards to it all interfaces except the source
        - G0/1 - PC4 ignores it because it does not match the IP address
        - G0/0 - PC3 sees the request and sends a reply
          - because the destination IP matches its IP address
- Reply to PC1 from PC3 - ARP reply message above
  - SW2 receives the ARP reply and updates its MAC address table with the source MAC address
    - 0C:2F:B0:6A:39:00 - on interface G0/0 - dynamically learned MAC
      - this is because it is not on the MAC address table
    - it then checks the destination MAC and sees it has an entry and forwards it to the appropriate port
      - 0C:2F:80:11:9D:00 on interface G0/2
  - SW1 is connected to interface G0/2 on SW2, SW1 then process the reply
    - it receives the reply message and updates its MAC address table with the source MAC and appropriate interface
      - 0C:2F:B0:6A:39:00 - on interface G0/2
    - it then forwards the reply to the destination MAC address, because it has an entry in the MAC address
      - 0C:2F:80:11:9D:00 on interface G0/0
- PC1 receives the reply and updates its ARP table with PC3's MAC address