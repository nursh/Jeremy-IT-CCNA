# Routing Fundamentals

- Routing is the process that routers use to determine the path that IP packets should take over a network to their destination
- routers store routers to all their known destinations in a routing table
- when routers receive packets, they look at a routing table to determine the best route to forward the packet


## Routing methods

- 2 main routing methods
- Dynamic routing
  - when routers use a dynamic routing protocol such as OSPF to share information with each other and build the routing tables
- Static routing
  - when a network engineer/admin manually configures routes on the router


- What is a route?
  - it is an instruction to the router
  - To get packet to destination X
  - send it to the next hop Y
    - next-hop (the next router)
    - or if the destination is the router's own IP address, receive the packet yourself
    - or if the destination is directly connected to the router, send it to the destination