# Subnetting(VLSM)

- subnet 192.168.5.0/24 for 4 LANS
- from largest to smallest hosts


- calculate the ip address for the LAN 
  - starting from LAN2
  - then set the highest usable address for the router's interface on the LAN


# on the router

- enable 
  - enter privileged exec mode
- conf t
  - then to global config mode
- int g0/1
  - set the interface g0/1
  - replace g0/1 with the interface
- ip addr <ip-address> <subnet-mask>
  - the subnet mask you configured for the interface
- no shut
  - enable the interface
- do show ip interface g0/1(do sh int g0/1)
  - primary shows layer3 interface on an interface
  - the broadcast address of 255.255.255.255
    - functions the same as the broadcast address of the network it is attached to
    - it can be used for any network
    - the broadcast will only be in the network and doesn't leave the LAN

- you can switch the interface while in global config mode
  - int g0/0
    - while in global config mode, switch to g0/0
  - then do any configuration you want


# on the host

- set the default gateway ip address(the one on the router)
- set the ip address of the host