# Ping 

- a network utility that is used to test reachability
- measures round-trip time
  - from A to B, back to A
- uses two messages
  - ICMP echo request
    - must be a unicast
    - fires out ARP first to find out MAC address
  - ICMP echo reply
- 4 pings by default for windows devices
- 5 pings by cisco devices

- command: ping (ip-address)
  - eg. ping 192.168.2.1