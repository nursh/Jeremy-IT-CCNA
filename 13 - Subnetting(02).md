# Subnetting - Part 2

### Create a subnet for 5 networks

- given 192.168.255.0/24 and create 5 sub-networks
- to calculate the number of subnets
  - 2^x = number of subnets
  - x = number of borrowed bits after the 24
- if you borrow 3 bits, you get 2^3 => 8 subnets
  - we can't have 5 equally
    - if you borrow 2, you get 4 subnets which is less than the desired number
  - 24 + 3(borrowed bits) = 27
- to get the subnet addresses beginning - the binary bits are the subnet starting address, i.e. the 3 borrowed bits
  - 24 + 000 + no of host bits
  - 24 + 001 + no of host bits
  - ...
  - 24 + 111 + no of host bits


### what subnet does host 192.168.5.57/27 belong to?

- change the IP to binary
  - 11000000.10101000.00000101.001 -> network | 11001 -> host
  - change all the host bits to 0, this will give you the network address
    - 192.168.5.32/27 -> is the subnet


### What subnet does host 192.168.29.219/29 belong to?

- change the ip to binary
- 11000000.10101000.00011101.11011 -> network | 011 -> host
- change all host bits to zero, convert the rest back to decimal
  - 192.168.29.216/29



### class b subnetting

- 172.16.0.0/16
- create 80 subnets for you companys various LAN
- 2^x -> to find how many subnets
  - 2^7 -> 128 subnets
  - borrow 7 bits
  - 255.255.254.0 -> subnet mask
- to know the subnets
  - look at the last bit set on the subnet mask
  - check what value it sets in binary
  - 0000 0010 -> the one sets the 2 bit
  - keep adding 2 to find all the subnets
    - if it was the 4 bit, keep adding 4
- our subnets will be
  - 172.16.0.0/23
  - ..2.0/23
  - ..4.0/23
  - ..6.0/23



- 172.22.0.0/16
- create 500 subnets for a company
- 2^x -> to find how many subnets
  - borrow 9 bits
  - 2 ^ 9 = 512 subnets
  - 255.255.255.128 -> subnet mask
  - 16 + 9 = 25 for the network
- our subnets will be
  - keep adding 128 to see each subnet
  - 172.22.0.0/25
  - 172.22.0.128/25
  - 172.22.1.0/25
  - 172.22.1.128/25


- 172.18.0.0/16 network
- 250 subnets with same number of hosts per subnet
- what prefix length 
- 2 ^ x -> to find how many subnets
  - borrow 8 bits
  - 2 ^ 8 = 256 subnets
  - 255.255.255.0 -> subnet mask
- our subnets will be having 256 hosts - 2(network and broadcast)
  - keep adding 256 to see each subnet
  - 172.22.0.0/24
  - 172.22.1.0/24
  - 172.22.2.0/24
  - ...
  - 172.22.255.0/24


- what subnet does host 172.25.217.192/21 belong to?
- find the subnet mask and zero out the hosts
  - 10101100.00011001.11011001.11000000
  - 11111111.11111111.11111 -> network | 000.00000000 -> host
  - convert 11011000 in the third octet -> 216
    - 172.25.216.0/21 network

## Practise questions

- 172.30.0.0/16 network
- should have 100 subnets with 500 hosts per subnet
- 2 ^ x -> subnets
  - borrow 7 bits - 2 ^ 7 -> 128 subnets
  - 255.255.254.0 -> subnet mask
  - 16 + 7 = /23
- no of hosts is 9 bits
  - 2 ^ 9 - 2 -> 510 hosts


- 172.21.111.201/20 belongs to which subnet
- find the subnet mask and zero out the hosts
  - 10101100.00010101.01101111.11001001
  - 11111111.11111111.1111 -> network | 0000.00000000 -> hosts
- subnet 172.21.96.0/20


- 192.168.91.78/26 belongs to which subnet
- find the broadcast address
  - 11000000.10101000.01011011.01001110
  - 11111111.11111111.11111111.11 -> network | 000000 -> hosts
  - network is 192.168.91.64/26
  - change all host bits to 1 for the last octet, that gives you the broadcast
    - 0111 1111 -> 127
- Broadcast address is 192.168.91.127/26

- divide 172.16.0.0/16 into 4 subnets of equal size
  - identify the network and broadcast of the second subnet
- 2 ^ x to divide into subnets
  - borrow 2 bits
  - 2 ^ 2 = 4 subnets
  - network becomes -> 172.16.0.0/18
  - subnet mask is 255.255.192.0
- keep adding one to the borrowed bits to get your next subnet value
  - keep the hosts as all 0 for network address
  - keep the hosts as all 1 for broadcast address
  - since be borrowed 2 bits in the 3rd octet(converted the 3rd octet to binary)
    - 172.16.00... -> 172.16.0
    - 172.16.01... -> 172.16.64
    - 172.16.10... -> 172.16.128
    - 172.16.11... -> 172.16.192
- second subnet will be 
  - 172.16.64.0/18 -> network address
  - 172.16.127.255/18 -> broadcast address


- divide 172.30.0.0/16 into subnets of 1000 hosts
  - how many subnets did you make?
- to get 1000 hosts
  - 2^x - 2 => closest to 1000 hosts
  - x should be 10 bits
  - 32 - 10 -> 22 for network
- 172.30.0.0/22 for a thousand hosts
- borrowed bits = network bits - given bits
- subnets = 2 ^ borrowed bits
  - 22 - 16 = 6
  - 2 ^ 6 subnets = 64 subnets