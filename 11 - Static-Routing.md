# Static Routing

- Static routes enable routers to send packets to destinations not directly connected to the router itself
  - remote networks
- NOTE: you can't configure a network address on an interface

## Default gateway 

- gateway is an old name for router
  - so default gateway simply means router
- say
  - PC1 is connected to 192.168.1.0/24
  - PC4 is connected to 192.168.4.0/24
- for both to send packets to destinations outside their local network
  - they have to send it to their default router
- in Linux systems it will look like this with eth0 as their interface
  - PC1 - iface eth0 inet static
            - address 192.168.1.10/24
            - gateway 192.168.1.1
  - PC4 - iface eth0 inet static
            - address 192.168.4.10/24
            - gateway 192.168.1.1

- the default gateway is also called default route
  - it is the least specific route
  - it is a route to 0.0.0.0/0
    - all netmask bits set to 0
    - all addresses from 0.0.0.0 to 255.255.255.255
    - it includes all IP addresses
  - what this means for PC1 and PC4 is that for any packets that do not have my IP address
    - send it to the default gateway
    - don't drop it

- setting a static route in a router can be done in 3 ways
  - next-hop
  - exit-interface
  - next-hop and exit-interface

## Use case

- send a packet from PC1 to PC4
  - but we don't know the MAC address of PC4
- packet
Src. IP -> 192.168.1.10
Dst. IP -> 192.168.4.10

- send packet to R1 on interface G0/2
  - using PC1 interface eth0
- use ARP to get mac address
- R1 has routes on the following interfaces
  - G0/2
    - 192.168.1.0/24 -> connected
    - 192.168.1.1/32 -> local
  - G0/1
    - 192.168.12.0/24 -> connected
    - 192.168.12.1/32 -> local
  - G0/0
    - 192.168.13.0/24 -> connected
    - 192.168.13.1/32 -> local
  - there is no route to 192.168.4.0/24
    - so it will drop the packet with the following config
- there are 2 possible paths to take to PC4
  - PC1 -> R1 -> R3 -> R4 -> PC4 - Path 1
  - PC1 -> R1 -> R2 -> R4 -> PC4 - Path 2
  - we want to use path 1 through R3
  - we can configure the router to choose a path for a packet
    - path 1 as main, path 2 as backup
    - load balance the path to prevent congestion
- remember routes are instructions
  - to send a packet to destinations in network 192.168.4.0/24, forward the packet to next hop Y

- R1 has no direct path R4 and R4 has no direct path to R1
  - there is R3 which is in between them

- These static routes have to be configured for PC1 and PC4 

Router | Destination.    | Next Hop
-------|-----------------|-------------
R1     | 192.168.1.0/24. | Connected
       | 192.168.4.0/24  | 192.168.13.3 -> R3
R3.    | 192.168.1.0/24. | 192.168.13.1 -> R1
       | 192.168.4.0/24  | 192.168.34.4 -> R4
R4.    | 192.168.1.0/24  | 192.168.34.3 -> R3
       | 192.168.4.0/24. | Connected


## configure static routes

- command structure
  - ip route ip-address netmask next-hop


### Setting R1 with the route 

- setting up R1 with static route for 192.168.4.0
  - ip route 192.168.4.0 255.255.255.0 192.168.13.3
    - to get to the network 192.168.4.0/24, use the route 192.168.13.3
- to show the routes, use do show ip route
  - S 192.168.4.0/24 [1/0] via 192.168.13.3
    - S is for static
    - [1/0] is for [Administrative Distance/Metric]
    - 192.168.13.3 is the route to use


### setting R3 with the routes

- ip route 192.168.1.0 255.255.255.0 192.168.13.1
  - to get to tne network 192.168.1.0/24, use the route 192.168.13.1
- ip route 192.168.4.0 255.255.255.0 192.168.34.4
  - to get to tne network 192.168.4.0/24, use the route 192.168.34.4


### setting R4 with the routes

- ip route 192.168.1.0 255.255.255.0 192.168.34.3
  - to get to the network 192.168.1.0/24, use the route 192.168.34.3


## Testing reachability 

- to test if PC1 can reach PC4 and back, use the ping command on PC1
  - ping 192.168.4.10
  - if PC4 can't reach PC1, it will fail
  - both machines are connected


## How the packet travels from PC1 to PC4 summary

- Hop 1 - PC1 to R1
- Src IP: 192.168.1.10
- Dst IP: 192.168.4.10
- Dst MAC: R1 G0/2 MAC
- Src MAC: PC! eth0 MAC


- Hop 2 -> R1 to R3
- Src IP: 192.168.1.10
- Dst IP: 192.168.4.10
- Dst MAC: R3 G0/0 MAC
- Src MAC: R1 G0/0 MAC


- Hop 3 -> R3 to R4
- Src IP: 192.168.1.10
- Dst IP: 192.168.4.10
- Dst MAC: R4 G0/1 MAC
- Src MAC: R3 G0/1 MAC

- Hop 4 -> R4 to PC4
- Src IP: 192.168.1.10
- Dst IP: 192.168.4.10
- Dst MAC: PC4 eth0 MAC
- Src MAC: R4 G0/2 MAC
- this is the only place where the Dst IP is the same as the Dst MAC

- Src IP and Dst IP never change throughout the journey


## setting a static route using exit interface

- using R2
- you can use any one you want


### using only exit-interface

- ip route ip-address netmask exit-interface
- ip route 192.168.1.0 255.255.255.0 g0/0
  - to get to the 192.168.1.0 network, use the g0/0 interface
- on the router, it shows that the static route is directly connected
  - but it is not
  - it relies on a feature called Proxy ARP to function 


### using exit-interface and next-hop

- ip route ip-address netmask exit-interface next-hop
- ip route 192.168.4.0 255.255.255.0 g0/1 192.168.24.4
  - to get to the 192.168.4.0 network, use the g0/1 interface which connected to 192.168.24.4 hop
  

## Default Route

- Gateway of last resort is another name for default gateway
- A default route is a route to 0.0.0.0/0
  - it is the least specific route possible, it includes all possible destination IP addresses
- if the router doesn't have any more specific routes that match a packet's destination IP address
  - the router will forward the packet using the default route
- A default route is often used to direct traffic to the internet
  - more specific routes are used for destinations in the internal corporate network
  - traffic to destinations outside of the internal network is sent to the internet
- if it says gateway of last resort is not set on a router
  - it means the default gateway is not set
  - ie. show ip route


### setting default route

- same as setting a static route but the ip address and netmask are all zeroes
ip route 0.0.0.0 0.0.0.0 <ip-address>
ip route 0.0.0.0 0.0.0.0 203.0.113.2
  - S* -> static route that is a candidate for being a default route
  - Gateway of last resort is 203.0.113.2 to network 0.0.0.0 