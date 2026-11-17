# Setting up routing

## setting up R1

- enter config mode and set up 3 of its interfaces

- conf t 
  - enter configuration mode
- interface g0/0
  - setting for interface g0/0
- ip address 192.168.13.1 255.255.255.0
  - set its ip address and subnet mask
- no shutdown


- interface g0/1
  - setting for interface g0/1
- ip address 192.168.12.1 255.255.255.0
  - set its ip address and subnet mask
- no shutdown

- interface g0/2
  - setting for interface g0/1
- ip address 192.168.1.1 255.255.255.0
  - set its ip address and subnet mask
- no shutdown
  - enable the router

- show ip int br
  - show interfaces 

- show ip route
  - display routing table of a router
  - codes
    - what routing protocols does a router use
    - L -> local
      - a route to the actual IP address configured on the interface
      - it has a /32 netmask
    - C -> connected
      - a route to the network the interface is connected to
      - it has the actual netmask configured on the interface
  - The output is not routes, it just tells you that for example
    - 192.168.1.0/24 is variably subneted, 2 subnets, 2 masks
      - there are 2 routes that fit within the 192.168.1.0/24 Class C network
      - it has 2 different netmasks(/24 and /32)

- when you add an IP address to an interface of a router
  - 2 routes will be added automatically
    - local
    - connected
- Local and connected routes are not static or dynamic
  - they are added automatically to the router when an IP addr is added on the interface


## Connected route

- a route to the network the interface is connected to
- R1 G0/2 IP -> 192.168.1.1/24
- 192.168.1.0 is the network address, 24 is the network part, the remaining 8 bits is the host
  - this provides a route to all hosts in that network
    - eg 192.168.1.10, 192.168.1.200 etc
- R1 now knows any packet meant for that network, needs to go through G0/2 interface


## Local route

- a route to the exact IP address configured on the interface
- /32 is used to specify the exact IP on the interface
  - all bits are fixed, there is no host part
- this tells it that if you receive a packet on this IP address
  - it is meant for this router specifically
- it is to my own IP adddress

- a route matches a packet's destination if the packet's destination IP address is part of the network specified in the route
- if a packet does not match any routes, the packet is dropped or


## Route Selection

- Routing chooses the most specific matching route with the longest prefix length
- if R1 receives a packet with Dst IP: 192.168.1.1
  - it matches both routes
    - 192.168.1.0/24
      - it includes 256 different IP addresses (..1.0 -> ..1.255)
    - 192.168.1.1/32
      - this has only 1 IP address
      - it is more specific
- R1 will keep the packet because it is more specific with the longest prefix length
- Local route -> it will keep the packet and encapsulate it