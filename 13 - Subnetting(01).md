# Subnetting - Part 1

- CIDR(classless-inter-domain-routing)

## IPv4 Address classes

- the x can either be a 1 or 0
- class A
  - 0xxx xxxx
  - 0 - 127 
  - 0.0.0.0 - 127.255.255.255
  - prefix length - /8 for network, /24 for hosts

- class B
  - 10xx xxxx
  - 128 - 191
  - 128.0.0.0 - 191.255.255.255
  - prefix length - /16 for network, /16 for hosts

- class C
  - 110x xxxx
  - 192 - 223
  - 192.0.0.0 - 223.255.255.255
  - prefix length - /24 for network, /8 for hosts

- class D 
  - 1110 xxxx
  - 224 - 239
  - 224.0.0.0 - 239.255.255.255

- class E
  - 1111 xxxx
  - 240 - 255
  - 240.0.0.0 - 255.255.255.255

- only class A, B and C can be assigned to routers
  - D and E have special uses


- IP addresses are assigned to companies and networks by IANA
  - IANA(Internet Assigned Numbers Authority) assigns IPv4 addresses/networks to companies based on their size
  - a large company might receive a class A or B network
  - a small company might receive a class C network
  - this led to many wasted IP addresses
- point to point network -> example of wasting IP addresses
  - network connecting 2 points
  - eg. router to router
    - use class C address
      - 203.2.112.0/24 is assigned to it
      - has 256 hosts 2^8 -> 256 hosts
      - 203.2.112.0 -> network address
      - 203.2.112.255-> broadcast address
      - 203.2.112.1 -> Router 1 address
      - 203.2.112.2 -> Router 2 address
      - you have used only 4 addresses and have wasted 252 addresses
        - this is why CIDR was created
- Another example of waste
  -  a company with 5000 employees
  -  can't use class C address because 2^8 -> 256 hosts
  -  has to use class B address 2^16 -> over 60000 hosts
     -  which is also a waste of addresses
- IETF introduced CIDR in 1993 to replace class system
  - remove class A => /8, class B => /16 and class C => /24
  - allow larger networks to be split into smaller networks
  - these smaller networks are called subnetworks or subnets


## Splitting a larger network into a smaller network

- 203.0.113.0/24
  - 1100 1011.0000 0000.0111 0001.0000 0000
- 255.255.255.0 - subnet mask
  - 1111 1111.1111 1111.1111 1111.0000 0000
- usable addresses
  - 2^8 - 2 -> 254 usable addresses
    - network address and broadcast address are not usable
      - hence the minus 2
    - 8 -> bits for host
- 2^n - 2 = usable addresses
  - n => number of host bits
- calculate addresses with different masks
  - 32 -> total number of bits
  - host bits = total number of bits - network bits
  - /25 -> 255.255.255.128
    - 32 - 25 -> 7 (host bits)
    - 2^7 - 2 -> 126 usable addresses
  - /26 -> 255.255.255.192
    - 32 - 26 = 6 (host bits)
    - 2^6 - 2 -> 62 usable addresses
  - /27 -> 30 usable addresses
  - /28 -> 14 usable addresses
  - /29 -> 6 usable addresses
  - /30 -> 2 usable addresses
  - /32 -> -1 usable addresses
    - useful for static routing to hosts
    - not for an interface
- we only need 2 addresses
  - 203.0.113.0/30
  - 32 - 30 = 2
  - 2^2 - 2 = 2 usable addresses
  - 203.0.113.0 - 203.0.113.3
    - network address - first .0
    - broadcast address - last .3
    - then our 2 usable addresses
  - 203.0.113.4 - 203.0.113.255 are available to be used
- /31 - for point to point connection
  - 32 - 31 = 1
  - 2^1 - 2 = 0 usable addresses
    - but 2 addresses are on there
  - this is perfect for what is needed
    - because it is a point to point connection
    - one router to the other
    - there is no need for a network address or a broadcast address
  - there is no waste
    - 203.0.113.0
    - 203.0.113.1
  - 203.0.113.2 - 203.0.113.255 are still available


## Designing a subnet - with 4 networks

- 4 networks with 45 hosts are given 192.168.1.0/24
  - each network is connected to a switch
  - each switch is then connected to a router
- each network has 45 hosts
  - one for the network
  - one for the broadcast
  - in total, we need 47 addresses
- we need a /26 subnet
  - 32 - 26 = 6
  - 2^6 - 2 => 62 usable addresses
  - which is more than enough
- 1st network -> 192.168.1.0/26 - .63/26
- 2nd network -> 192.168.1.64/26 - .127/26
- 3rd network -> 192.168.1.128/26 - .191/26
- 4th network -> 192.168.1.192/26 - .255/26