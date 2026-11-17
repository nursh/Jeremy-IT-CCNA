# Life of a packet lab

- The switch does not touch the ethernet frame
  - it either floods the network to get the MAC of the router
  - or it forwards the packet to the interface connected to the router
- ping times out because all the devices are sending ARP request
  - before the ping works out, where all the addresses are resolved
- ping works 4 times in Windows computer by default
- checking the simulator that you understand the movement
- ipconfig \all
  - check the MAC address of an interface


## Check MAC address on router

- en
  - enter privileged exec mode
- show interfaces g0/0
  - show the details at interface g0/0 include MAC address
- you can give a router a mac address different for burnt-in mac address


## simulation

- use simulation to view packet
- open the simulation tab on the bottom
- ping PC4 from PC1
- look at the layers
  - layer 2 MAC address -> confirm the mac addresses
  - layer 3 -> IP addresses don't change
