# Switch Interfaces

- Interface speed and duplex
  - bits per second
  - both and receive data at the same time
- speed and duplex autonegotiation
- interface status
- interface counters & errors

- command: show ip interface brief
  - show interfaces with other info on switch
    - like IP address on interface
- switches don't have the shutdown command
  - if you connect them to another device, they will be in the up/up state with no configuration required
  - no IP address is assigned
    - layer 2
- down/down is different from administratively down/down
  - the admin down/down is because of shutdown command on routers
    - which is by default
- switches config
  - down/down -> not connected to any device
  - up/up -> connected to a device
- command: show interfaces status
  - port -> show all interfaces
  - name -> description of interfaces
  - status -> connected/notconnect
  - vlan -> divide lans into smaller lans
  - duplex -> send and or receive data
    - auto(default) -> negotiate with neighbouring device
    - a-full -> negotiate for full duplex
  - speed -> speed to send data
    - auto(default) -> negotiate speed with device
    - a-100 -> 100Mbits per second
  - type -> type of connector


## configure speed and duplex

- conf t
  - enter interface configuration mode
- int f0/1
  - setup for interface f0/1
- speed ?
  - see which speeds are available
- speed 100
  - select speed for 100Mbps
- duplex ?
  - which duplex styles are available
- duplex full
  - set it to full duplex(send and receive)
- description <some-description>
  - set the name field with the description


## disable unused interface

- from global config mode
- interface range f0/5 - 12
  - enter interface range config mode
  - setup from interfaces f0/5 - 12
- description ## not in use ##
  - set the description to ## not in use ##
- shutdown
  - set the state to administratively down

- to set interfaces not consecutively
- int range f0/5 - 6, f0/9 - 12
  - get interface 5, 6, 9, 10, 11, 12
- no shut
  - set their state to up


## Full and Half duplex

- half-duplex
  - device can't send and receive data at the same time
  - if it is receiving a frame, it must wait before sending a frame
  - used almost no where
  - it was used in hubs(outdated devices - also called repeaters)
    - collision domains -> all devices connected with the hub
    - used CSMA/CD(carrier sense multiple access with collision detection) to avoid collisions
      - before sending frames, devices listen to collision domain until they detect that other devices are not sending
      - if a collision occurs, the device sends a jamming signal to inform other devices that a collision happened
      - each device will wait a random period of time before sending frames again
      - the process repeats
- full duplex
  - the device can send and receive data at the same time, it doesn't have to wait
  - used by switches
  - this is the preferred way
  - collisions occur but they are rare
    - usually due to misconfiguration
  - each device with the device is now a collision domain


## Speed / Duplex Autonegotiation

- interfaces that can run at different speeds have default settings of speed auto and duplex auto
  - 10/100 - F
  - 10/100/1000 - G
- interfaces advertise their capabilities to the neighboring device
  - then they negotiate the bast speed and duplex settings they are capable of
- What if autonegotiation is disabled on the device connected to the switch
  - speed -> the switch will try to sense the speed that the other device is operating at
    - if it fails to sense the speed, it will use the slowest supported speed
    - i.e 10Mbps on a 10/100/1000 interface
  - duplex
    - if the speed is 10 or 100Mbpx, the switch will use half duplex
    - if the speed is 1000Mbps or greater, use full duplex
- make sure to use autonegotiation on your devices
  - this will avoid collision and configuration mismatch
  - if the switch decides to use half duplex but the device support full duplex
    - the full duplex device will send data even when the switch is not ready to receive it
    - this is what causes the collision


## See errors at interfaces

the same errors exist on the router like the switch
- show interfaces <interface>
  - show errors on interface
- show interfaces f0/2
  - show errors that happened at interface f0/2
- runts
  - frames that are smaller than the minimum frame size(64 bytes)
- giants
  - frames that are larger than the maximum frame size(1518 bytes)
- CRC
  - frames that failed the CRC check(in the ethernet FCS trailer)
- input errors
  - total of various counters, such as the above four
- output errors
  - frames the switch tried to send, but failed due to an error