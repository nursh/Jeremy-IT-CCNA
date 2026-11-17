# IPv4 Header

- Routing 
  - sending messages from one host to another host in another network

## Protocol Data Units(PDUs)

- Data
  - application data
- Data + L4 header -> segment
  - TCP or UDP
- Data + L4 header + L3 header -> Packet
  - IP 
- L2 trailer + Data + L4 header + L3 header + L2 header -> Frame
  - Ethernet frame

- we are interested in Layer 3 and routing


## Fields of the IPv4 header

- version - 4 bits
  - identifies the version of IP used
  - 0100 - IPv4
    - this is what we are looking at
  - 0110 - IPv6
    - has a different structure

- internet header length - 4 bits
  - describes the total length of the header
    - the final field of the IPv4 header (options) is variable in length
  - it identifies the length in 4 byte increments
    - if the value is 5, the length of the header is
      - 5 x 4 bytes = 20 bytes
  - the minimum value is 5 and maximum value is 15
    - if the value is 15 -> 1111
    - the length will be 15 x 4 bytes -> 60 bytes
  - minimum length of IPv4 header is 20 bytes while the max is 60 bytes

- DSCP field - 6 bits
  - Differentiated Services Code point
  - used for QoS(Quality of service)
  - used to prioritize delay-sensitive data(streaming voice, video etc)
  - used to check which service should receive priority based on quality of internet

- ECN - 2 bits
  - Explicit Congestion Notification
  - provides end-to-end notification of network congestion without dropping packets
  - it is an optional field that requires both endpoints
    - as well as the underlying network infrastructure to support it

- total length - 2 bytes - 16 bits
  - indicates the total length of the packet including the (L3 header + L4 segment)
  - measured in bytes
  - minimum of 20
    - only the IPv4 header
  - maximum of 65535

- identification field - 2 bytes - 16 bits
  - if a packet is fragmented due to being too large, this field is used to identify which packet the fragment belongs to
  - all fragments of the same packet will have the same value in this field
  - packets are fragmented if larger than the MTU(Maximum Transmission Unit) - usually 1500 bytes
  - fragments are reassembled by the receiving host

- flags - 3 bits
  - used to control/identify fragments
  - bit 0 -> reserved(aka not set), always set to 0
  - bit 1 -> don't fragment (DF bit)
    - do not fragment the packet
  - bit 2 -> more fragments(MF bit)
    - set to 1 if there are more fragments in the packet
    - it is set to 0 for the last fragment
    - if is also set to 0 if the packet is unfragmented

- Fragment offset -> 13 bits
  - used to indicate the position of the fragment within the original unfragmented IP packet
  - allows fragmented packets to be reassembled even if they arrive out of order

- Time to live -> 8 bits (1 bytes)
  - used to prevent infinite loops
  - a router will drop a packet with a TTL of 0
  - it was originally designed to indicate the packet's maximum lifetime in seconds
    - in practice, it indicates a hop count, each time the packet arrives at a router, the router decreases the TTL by 1
  - recommened default is 64

- Protocol - 8 bits - 1 byte
  - it indicates the protocol of the encapsulated L4PDU
  - some common and useful values
    - value of 6: TCP
    - value of 17: UDP
    - value of 1: ICMP
    - value of 89: OSPF (dynamic routing protocol)

- Header Checksum - 2 bytes - 16 bits
  - calculated checksum used to check for errors in the IPv4 header
  - when a router receives a packet, it calculates the checksum of the header and compares it to this field header
    - if they do not match, the router drops it
  - only checks for errors in the IPv4 header
  - encapsulated protocol is used to detect errors in the encapsulated data
  - TCP and UDP have their own checksum fields to detect errors in the encapsulated data

- Source and Destination address - 32 bits each (4 bytes each)
  - source -> sender
  - destination -> intended receiver
  
- options field (0 - 320 bits) - 0 - 40 bytes
  - it is rarely used
  - if the IHL is greater than 5, then it has some values

