# The network used

- WAN - wide area network
  - network that extends over a large geographical area


## LAN 1

- network address - 192.168.1.0/24
PC1 -> .10
  -> has ethernet interface of eth0
SW1


## LAN 2

- network address - 192.168.4.0/24
PC4 -> .10
  -> has ethernet interface of eth0
SW2


## Routers and their interfaces

- the number below is the IP address of the router on the network configured on the interface
- R1
  - SW1 -> G0/2 -> 192.168.1.0/24
    - 192.168.1.1
  - R2 -> G0/1 -> 192.168.12.0/24
    - 192.168.12.1
  - R3 -> G0/0 -> 192.168.13.0/24
    - 192.168.13.1

- R2
  - R1 -> G0/0 -> 192.168.12.0/24
    - 192.168.12.2
  - R4 -> G0/1 -> 192.168.24.0/24
    - 192.168.24.2

- R3
  - R1 -> G0/0 -> 192.168.13.0/24
    - 192.168.13.3
  - R4 -> G0/1 -> 192.168.34.0/24
    - 192.168.34.3

- R4
  - R2 -> G0/0 -> 192.168.24.0/24
    - 192.168.24.4
  - R3 -> G0/1 -> 192.168.34.0/24
    - 192.168.34.4
  - SW2 -> G0/2 -> 192.168.4.0/24
    - 192.168.4.4


## Connection Diagram

PC1 --> SW1 --> R1 --> R2
                |.     |
                |.     |
                |.     |
                R3 --> R4 --> SW2 --> PC2
