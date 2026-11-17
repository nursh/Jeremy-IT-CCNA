# Lab 9

## on the router

- enable / en
  - enter config mode
- configure terminal / conf t
  - configure terminal
- hostname R1
  - change the name of the router to R1
- int g0/0
  - enter interface config mode for g0/0
  - use the g0/0 for the next configs
- ip address 172.15.255.254 255.255.0.0
  - 255.255.0.0 is the subnet mask of /16
  - set the IP address of g0/0
- speed 1000
  - set the speed to 1000bps
- duplex full
  - use duplex full
- description ## to SW1 ##
  - set the description for the interface
- do show ip int brief
  - do allows you to use show command
  - when IP address is unassigned, the Method is unset
  - shows the interfaces on the router
- no shutdown
  - enable the interface 
  - all other interface are disabled by default because it is a router
- int range g0/1 - 2
  - make the setting for g0/1 and g0/2
- description ## not in use ##
  - set the above interface descriptions to not in use
- do show run
  - see config of device
- end
  - go to privileged exec mode
- copy running-config startup-config
  - save the config
- show startup-config
  - see the save config 


## setting the IP of a PC

- click on it
- go to the config setting on the top
- on the left panel
  - click fastethernet0
- in the main panel, you will see IPv4
  - set the address there


## configure switch

- enable
- conf t
- host SW1
  - change hostname to SW1
- do show int status
  - do show interfaces status
  - only works on switches
  - statuses are either connected or not connected
    - they are not shutdown by default, that is only for routers
  - speed and duplex are auto by default

### setting switch interfaces to router and other switch

- int g0/1
  - select interface g0/1
- speed 1000
  - set speed to 1000bps
- duplex full
  - use full duplex mode
- description ## to R1 ##
  - set description (in the table, the name field ) to "## to R1 ##"
- repeat the last 4 commands with interface g0/2
  - but instead the description should be for ## to SW2 ##

### setting switch interfaces to end hosts

- int range f0/1 - 2
  - connect to interfaces f0/1 and f0/2
- description ## to end hosts ##
  - set both their descriptions to "## to end hosts ##"


### shutdown the unused interfaces

- int range f0/3 - 24
  - connect to interfaces f0/3 - f0/24
- description ## not in use ##
  - set their description to ## not in use ##
- shutdown
  - shutdown all the interfaces administratively
  - it will disable them


### shutdown the unused interfaces in SW2

- int range g0/2, f0/3 - 24
  - select ports g0/2 and f0/3 to f0/24


### come out and save

- end 
  - come out of configuration mode
- write mem
  - save the configuration
- show startup-config
  - show the configurations