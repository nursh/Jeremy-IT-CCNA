# Subnetting

- Designing for hosts in a subnet
  - given a number of hosts 
  - (2 ^ x) - 2 = no of hosts
  - find x, the closest number of hosts 
  - if we want 10 hosts
  - 2 ^ 4 - 2 = 14 hosts
    - (-2) bcos of network address and broadcast address
  - so we need to reserve 4 bits for hosts
  - so the subnet mask is 32 - 4 = 28
    - 28 bits for the network
    - 4 bits for the hosts


- designing for subnets
  - 2 ^ x = no 
  - if we want 10 subnets
  - 2 ^ 4 = 16 subnets 
    - closest number to 10 subnets
  - so we have to reserve 4 extra bits for the network
  - if given /16
    - the subnet mask will now be 16 + 4 = 20
  - the subnet address wlll now be
  - /16 bits for the original + 0000....1111 for the added subnet and the rest for the hosts

- to get network address
  - zero out all host bits

- to get broadcast address
  - set all host bits

- the art of borrowing
  - class C can borrow up to 8 bits due to /24
  - class B up to 16 bits - /16
  - class A up to 24 bits - /8