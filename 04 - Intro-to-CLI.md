# Intro to the CLI

- About Cisco IoS CLI
  - operating system used on cisco devices
- CLI - command line interfaces
  - used for routers, switches etc
- GUI - graphical user interface


## How to connect to a Cisco device to configure

- using the console port on device to a laptop
  - it has 2 port you can choose from
    - RJ45
      - using a rollover cable(using 8 pins)
        - pin 1 to pin 8, 2 to 7 up until pin 1 to pin 8
      - use putty and the configurations for cisco are:
        - speed(baud) - 9600
        - for each 8 data bits, 1 stop bit is sent
        - parity used to detect errors - none
        - flow control - none
    - USB-mini-b


## Modes

- user EXEC mode or user mode
  - has > in the terminal next to the hostname of the device
    - Router>
    - Router is the hostname of the device
  - it is very limited
    - can look at things but can't make changes to the configuration
  - command: enable
    - switch to privileged EXEC mode

- privileged EXEC mode
  - has # sign in the termal next to the hostname
    - Router#
  - can view the configuration of the device, but cannot change them
    - but can change time on the device and save the configuration file

- global configuration mode
  - allows configuration
  - to allow configuration, switch from privileged mode, using the command below
    - command: configure terminal
      - shortcut: conf t
    - allow configuration
  - the terminal has "Router(config)" to show you the mode

- command: ?
  - view available commands for a mode
- using <tab> key helps with autocomplete
- to put password to stop people from configuring 
  - passwords are case-sensitive
  - in the global configuration mode
  - command: enable password ?
    - eg enable password CCNA
      - set the password to CCNA
- command: exit
  - when in global config mode
    - takes you to privilege mode
  - when in privileged mode
    - logs you out of the device
- command: run <privileged-exec-level-command>
  - execute a privileged-exec-command from global config mode


## Configuration files

- There are 2 separate configuration files kept on the device
- Running config
  - current, active configuration file on the device
  - as you enter commands in the CLI, you edit the active configuration
- Startup-config
  - config file that will be loaded upon restart of the device
- in privileged EXEC mode, use the command: show running-config
  - shows the running-config file
- to view the startup-config file
  - command: show startup-config
- To save config file(running configuration as start-up configuration)
  - in privileged EXEC mode, there are 3 ways
    - command: write
    - command: write memory
    - command: copy running-config startup-config
- the password shows up in the config file in clear text
  - to add security, enter global configuration mode
  - then use the command: service password-encryption
    - changes the display of password in the config file
    - it uses Cisco type-7 encryption, but it can be cracked
    - enable secret will not be effected
  - use the command: enable secret <secret>
    - eg enable secret Cisco
    - uses MD5 encryption
    - always
  - if enable secret and enable password are enabled
    - only enable secret can be used to login

- putting do in terminal lets you execute privileged mode command in other modes
  - eg do sh run
    - while in global configuration mode
    - show the running configuration


## Cancel a command

- use the word no in front of a command
  - command: no service password-encryption
    - future password will not be encrypted
    - but encrypted password will remain