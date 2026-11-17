# Subnetting - Part 3


## Subnetting class A


### 10.0.0.0.8

- 10.0.0.0/8
  - create 2000 subnets
  - what prefix length must you use
  - how many host addresses will be in each subnet

- 2 ^ x >= 2000 subnets
  - x = 11
  - 11 borrowed bits
  - 10.0.0.0/19 will be the prefix
  - 255.255.224.0 -> subnet mask

- host bits = 32 - 19 = 13 bits
  - 2 ^ 13 - 2 = 8190 addresses


### 10.217.182.223/11

- find the network address
  - 0000 1010.1101 1001.1011 0110.1101 1111
  - 1111 1111.1110 0000.0000 0000.0000 0000
  - address = 10.192.0.0/11
- broadcast address
  - broadcast address -> 10.223.255.255/11
- first usable address
  - address -> 10.192.0.1/11
- last usable address
  - address -> 10.223.255.254/11 
- no of host(usable addresses)
  - 32 - 11 = 21
  - 2 ^ 21 - 2 = 2,097,150


## VLSM(Variable-Length Subnet Mask)

- the previous examples were FLSM(Fixed Length Subnet Mask)
- VLSM
  - process of creating subnets of different sizes to make your use of network addresses more efficient


### Example of using sub-network

- Tokyo 
  - LAN A - 110 hosts
  - LAN B - 8 hosts
- Toronto
  - LAN A - 29 hosts
  - LAN B - 45 hosts
- Tokyo and Toronto have a point to point connection on their router
- 192.168.1.0/24
  - 5 subnets


### steps

- assign from largest to smallest VLAN until they have been assigned
  - Tokyo A - 7 bits for host
    - /25
  - Toronto B - 6 bits for host
    - /26
  - Toronto A - 5 bits for host
    - /27
  - Tokyo B - 3 bits for host
    - /28
  - Point to Point router - 2 bits for host
    - /30

- Tokyo A - uses half of 192.168.1.0/24 - 110 hosts
  - network address - 192.168.1.0/25
  - broadcast address - 192.168.1.127/25
  - first usable address - 192.168.1.1/25
  - last usable address - 192.168.1.126/25
  - total number of usable host addresses - 126 
- Toronto B - 45 hosts
  - network address - 192.168.1.128/26
  - broadcast address - 192.168.1.191/26
  - first usable address - 192.168.1.129/26
  - last usable address - 192.168.1.190/26
  - total number of usable host addresses - 62
- Toronto A - 29 hosts
  - network address - 192.168.1.192/27
  - broadcast address - 192.168.1.223/27
  - first usable address - 192.168.1.193/27
  - last usable address - 192.168.1.222/27
  - total number of usable host addresses - 30
- Tokyo B - 8 hosts (then add network and broadcast) = 8
  - network address - 192.168.1.224/28
  - broadcast address - 192.168.1.239/28
  - first usable address - 192.168.1.225/28
  - last usable address - 192.168.1.238/28
  - total number of usable host addresses - 14
- point to point connection - 2 hosts (then add network and broadcast) = 4
  - network address - 192.168.1.240/30
  - total number of usable host addresses = 2
  - broadcast address - 192.168.1.243/30
  - first usable - 192.168.1.241/30
  - last usable - 192.168.1.242/30
  - one for Tokyo and one for Toronto


## Practise Questions

- www.subnettingquestions.com
- subnetting.org
- subnettingpractice.com
  - very good