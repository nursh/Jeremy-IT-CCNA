# Setting up interface

- using gigabitethernet0/0 as an example
  - interface g0/0

- switch to privileged exec mode
  - en
- show the status of all interfaces on the router
  - show ip interface brief
- select the interface you want to set the IP address for 
  - interface <interface-value>
  - interface gigabitethernet0/0
- set the IP address
  - ip address <ip-address> <subnet-mask>
  - shortcut: ip add <ip-address> <subnet-mash>
  - ip address 10.255.255.254 255.0.0.0
- enable interface
  - no shutdown
  - shortcut: no shut
- check the status of the interfaces
  - using shortcut for showing interfaces on the router
  - do sh ip int br
    - the do is infront of the command because you are in configuration mode
    - show commands only work in privileged exec mode
- this set the status and protocol fields up
- save the configuration
  - write mem


## show interface

- show layer 1 and layer 2 information about an interface
- show interfaces [interface]
  - eg show interfaces g0/0
- shows a lot of information
  - mac address


- show interfaces description
  - determine description of interfaces


### Steps to set up description for an interface

- int g0/0
  - enter interface configuration mode
  - configure interface g0/0
- description ## to SW1 ##
  - set the description for interface g0/0 to be "## to SW1 ##"
  - but there are not rules for setting description
  - shortcut desc <message>