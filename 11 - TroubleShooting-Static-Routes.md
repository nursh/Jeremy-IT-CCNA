# Troubleshooting static routes

- PC1 and PC2 can't ping each other
  - find what is wrong with the router
  
- Check the configuration on the PC first
- use the ipconfig(Windows), ifconfig(Unix)
- or ipconfig /all
  - includes MAC/physical address

- try pinging the default gateway


## Checking the router configuration

- enable
  - enter privileged exec mode
- show ip interface brief
  - see all the interfaces
- show ip route
  - look at the routes
- show running-config | include ip route
  - shows configuration statements
  - ip route 192.168.3.0 255.255.255.0 192.168.12.3 
    - the correct hop is 192.168.12.2 not .3
- configure terminal
  - enter global config mode
- no ip route 192.168.3.0 255.255.255.0 192.168.12.3 
  - in global config mode
  - this cancels this configuration
- do show run | include ip route
  - make sure the wrong command has been removed
- ip route 192.168.3.0 255.255.255.0 192.168.12.2
  - fix the config
  - then check again with the do show run ... command
  

## to fix interface

- after conf t (configure terminal)
- int g0/0
  - i want to configure g0/0 interface
- ip add 192.168.13.3 255.255.255.0
  - set the address at interface g0/0 to 192.168.13.3
  - overwrite anything that was there before


- after fixing the routes, ping PC2 again


## Summary

- check if you can ping
- check the interfaces of the router
- check the routes configured on the router
- you must remove the wrong configuration statement before adding the correct
  - it will load balance the packets between the good path and the bad path
  - this is not what we want
  - use: no ip route ...
  - this is only for routes
- to fix interface ip, just put in the right address