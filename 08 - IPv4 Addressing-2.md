# IPv4 Addressing Part 2

- Given an address
  - 172.16.0.0/16 - 127.16.255.255/16
  - network id(address) - broadcast address
  - host part = 32 - prefix
    - 32 - 16 = 16
    - 2 ^ 16 - 2 -> gives the number of addressable hosts
      - the network address and the broadcast address are not assignable
- first usable address
  - add one to the network address
- last usable address
  - subtract one from the broadcast address


## Setting up IP addresses on a Cisco router

- en 
  - switch to privileged exec mode
- cisco routers interfaces are shut down by default
- show ip interface brief
  - show each interface and the IP address assigned to it
  - status column refers to the layer 1 status(physical layer)
    - is the interface shut down
    - cisco routers are shut down by default
    - because layer one is shutdown, the protocol column (layer 2) will also be shutdown
  - protocol column
    - layer 2 status
    - if ethernet enabled for both devices
- conf t
  - enter global config mode
- interface gigabitethernet 0/0
  - enter interface configuration mode and configure interface g0/0
  - shown by R1(config-if)#
    - in the terminal
  - shortcut:
    - int g0/0
- ip address <ip-address> <subnet-mask>
  - set ip address of a certain interface
  eg ip address 10.255.255.254 255.0.0.0
- no shutdown
  - enable interface
- do sh ip int br
  - show ip interface brief