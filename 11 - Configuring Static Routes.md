# Configuring Static Routes

- PC1 to PC2

## PC config

- set the IP of the PC
- then set the default gateway

- Click on config for PC1(after clicking it)
- set the gateway - R1
  - how the packets will leave PC1
  - R1's IP address
- set the interface
  - using fastethernet0
  - set the IP address and netmask of PC1 


## Router config

- click on the router you want to config
- select CLI
- enter: enable
  - to enter privileged exec mode
- enter: conf t
  - configure terminal
  - to enter global config mode
- enter: hostname R1
  - set the hostname to R1


### enable interfaces

- interface g0/1
  - i want to configure interface g0/1
- ip address 192.168.1.254 255.255.255.0
  - set the address on the port to the given IP with the netmask
- description ## to SW1 ##
  - add a description for the router
- no shutdown
  - enable interface
- do show ip interface brief
  - show the status of ihe interfaces on the router
  
- do the same steps for the remaining interfaces on the router

if R1 and R2 are connected and one of the interfaces is not set
  - the status will show up for the enabled one and down for the disabled one
  - just update the interface and it will be on


## configure static routes

- using R1 as an example
- Two way reachability
  - PC1 ping can reach PC2
  - PC2 reply can reach PC1
- you have to be in global config mode to set the next hop
  - use exit interface config mode
- ip route ?
  - this is the helper to tell you the option to put there
- ip route 192.168.3.0 255.255.255.0 192.168.12.2
  - to get to the network 192.168.2.0 with netmask 255.255.255.0
  - use the path 192.168.12.2
  - alternatively you could have used the interface with g0/0 to specify
- do show ip route
  - shows the routing table

- set the rest yourself


## Pinging

- pinging PC2 from PC1
- click on PC1
- click on command prompt
  - then ping
  - the first ping might fail because of ARP
  - then the rest will work