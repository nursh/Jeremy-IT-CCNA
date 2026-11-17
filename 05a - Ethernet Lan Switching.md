# Ethernet LAN Switching

- LAN(local area network)
  - network contained in a relatively small area
  - routers are used to connect LANs
  - switches do not separate LANs
- switches forward frames - PDUs in layer 2 
  - with header and trailer
- 5 fields in ethernet frame header
  - Preamble
  - SFD - Start Frame Delimiter
  - Destination
  - Source
  - Type - layer 3 protocol used in packet / sometimes it is the length field
    - IPv6
    - IPv4
- field in ethernet frame trailer
  - FCS - Frame Check sequence
    - check for errors


## Preamble

- length - 7 bytes long
  - 56bits
- series of alternation 1's and 0's
  - 10101010 * 7 -> 1 byte * 7
  - allow devices to synchronize their receiver clocks


## SFD

- start frame delimiter
- length - 1 byte
- value - 10101011
  - signifies end of preamble and beginning of the rest of the frame


## Destination and Source

- destination is 6 bytes
- source is 6 bytes
- indicate the devices sending and receiving the frame
- consist of the destination and source MAC address
- MAC address is a 6 byte of the physical device


## Type or length

- length 2 bytes 
- can either be type of encapsulated packet or its length
- if the value is 1500 or less 
  - it represents the LENGTH of the encapsulated packet in bytes
- if the value is 1536 or greater
  - it shows the TYPE of the encapsulated packet(usually IPv4 or IPv6)
  - the length is determined via other methods
- IPv4 - 0x0800 (hexadecimal) - 2048 (decimal)
- IPv6 - 0x86DD - 34,525
- ARP - 0x0806
  - shows an ARP packet is inside ethernet frame


## FCS

- 4 bytes in length
- detect corrupted data by running a CRC algorithm over the received data
- CRC - cyclic redundancy check


- The whole size are 26 bytes(header + trailer)



## MAC Address

- 6 byte assigned to the device when it is made
- aka Burned in Address (BIA)
- it is globally unique
  - there are locally unique ones
- first 3 bytes are the OUI(organizationally unique identifier)
- last 3 bytes are unique to the device 
- they are written in hexadecimal
- unicast
  - frame destined for a single target


- F0/2 -> interface on switch
  - F -> fast ethernet - 100Mb/s

## Sending a message in a LAN

- 3 PCs
  - PC1 - AA.AA.AA.00.00.01
  - PC2 - AA.AA.AA.00.00.02
  - PC3 - AA.AA.AA.00.00.03
- SW1 with three interfaces
  - F0/1 - connected to PC1
  - F0/2 - connected to PC2
  - F0/3 - connected to PC3

- if PC1 wants to send a message to PC2
  - it sends a frame to the switch with the following things
    - Destination: AA.AA.AA.00.00.02
    - Source: AA.AA.AA.00.00.01
  - the switch records the source information in a MAC address table with two columns
    - MAC - AA.AA.AA.00.00.01, interface - F0/1
    - the switch now knows which interface PC1 is connected to
      - dynamically (learned) MAC address because it wasn't programmed into the switch
      - the switch had to learn this by looking at source field in the ethernet frame
  - unknown unicast frame
    - a frame whose entry is not on the MAC address table of the switch
    - in this case this is our destination field in the frame
    - the switch doesn't know where to send it
  - FLOOD the frame
    - when a switch doesn't know where to send a frame
    - it sends a message to all its interfaces except the one it got the message from
    - in our case:
      - F0/2 and F0/3
      - but not F0/1
  - PC3 ignores the package because it wasn't meant for it
  - PC2 receives and processes the package
    - however if it does not reply to PC1
    - the switch never learns its interface and MAC address
    - if it has a reply, it sends a frame to the switch with the appropriate destination and source fields
  - known unicast frame - forward the frame
    - the destination for the reply is on the MAC Address Table of the switch
      - in our case PC1 on F0/1 interface
    - so there is no flooding of the frame, it is send directly to its recipient
- In Cisco switches
  - Dynamic MAC addresses are removed from the MAC address table after 5 minutes of inactivity


## sending a message with 2 switches connected

- in the format for the switch
  - interface - device(MAC address)
  - the MAC address has been shortened
    - AA.AA.AA.00.00.0x
    - where x is the number of the device
- SW1
  - F0/1 - PC1(01)
  - F0/2 - PC2(02)
  - F0/3 - SW2
- SW2
  - F0/1 - PC3(03)
  - F0/2 - PC4(04)
  - F0/3 - SW1

- if PC1 wants to send a message to PC3
  - it sends a frame with:
  - destination(03)
  - source(01)
- SW1 updates its MAC addressing table with the source information it got from the frame
  - MAC - 01, interface - F0/1
- SW1 does not know the interface for the destination, so it floods the other interfaces
  - unknown unicast frame
  - it sends the frame to F0/2 and F0/3 but not F0/1(sender of message)
  - F0/2 ignores the message because it isn't meant for it
- SW2 receives the message and stores the source information in its MAC addressing table
  - MAC - 01, interface - F0/3
  - but it has no address for the destination MAC
- SW2 floods its interfaces with the message(unknown unicast frame)
  - sends it to F0/1 and F0/2 but not F0/3(sender of message)
- PC4 on interface F0/2 ignores the message
- PC3 on interface F0/1 receives the message
  - it accepts the message, processes it and sends a reply with the information
  - source - 03
  - destination - 01
- SW2 receives a message and stores the source information on its MAC addressing table
  - MAC - 03, interface - F0/1
  - then it forwards the message to the interface of the destination because it was stored earlier
    - MAC - 01, interface - F0/3
- SW1 receives the message because it is on F0/3 of SW2
  - it updates its MAC addressing table with the new information
  - MAC - 03, interface - F0/3
  - it then forwards the message to the destination as it already exists on the MAC table
    - MAC - 01, interface - F0/1