# IP Addressing Plan
The network uses the private IPv4 network:

192.168.1.0/24

Subnet mask: 255.255.255.0

Default Gateway: 192.168.1.1

# Table Addressing
| Device        | Interface | IP Address  | Subnet Mask   | Default Gateway |
| ------------- | --------- | ----------- | ------------- | --------------- |
| Office-R1     | G0/0      | 192.168.1.1 | 255.255.255.0 | N/A             |
| PC1-Reception | Fa0       | DHCP        | 255.255.255.0 | 192.168.1.1     |
| PC2-Reception | Fa0       | DHCP        | 255.255.255.0 | 192.168.1.1     |
| PC3-Sales     | Fa0       | DHCP        | 255.255.255.0 | 192.168.1.1     |
| PC4-Sales     | Fa0       | DHCP        | 255.255.255.0 | 192.168.1.1     |
| PC5-Manager   | Fa0       | DHCP        | 255.255.255.0 | 192.168.1.1     |


# DHCP Configuration
The router provides DHCP services.

# The DHCP pool is:
    OFFICE-LAN

# Network:
    192.168.1.0/24

# Default Gateway:
    192.168.1.1

# DNS server:
    8.8.8.8

# Note:
    The following addresses are excluded from DHCP:
    192.168.1.1 - 192.168.1.20
    
    This leaves: 
    192.168.1.21 - 192.168.1.254
    Available for dynamic assignment.


# Router Configuration
The router is configured as the default gateway and DHCP server.

# Basic Configuration:
enable
configure terminal

hostname Office-R1

no ip domain-lookup

enable secret class

service password-encryption

banner motd #UNAUTHORIZED ACCESS PROHIBITED#

interface GigabitEthernet0/0
description Connection-to-Office-SW1
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

# DHCP Configuration
ip dhcp excluded-address 192.168.1.1 192.168.1.20

ip dhcp pool OFFICE-LAN
description Office DHCP Pool
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
dns-server 8.8.8.8
exit
end

copy running-config startup-config

# Switch Configuration

The switch provides Layer 2 connectivity between the router and PCs.

enable
configure terminal

hostname Office-SW1

no ip domain-lookup

enable secret class

service password-encryption

banner motd #UNAUTHORIZED ACCESS PROHIBITED#

interface GigabitEthernet0/1
description Connection-to-Office-R1
switchport mode access
no shutdown
exit

interface FastEthernet0/2
description PC1-Reception
switchport mode access
spanning-tree portfast
no shutdown
exit

interface FastEthernet0/3
description PC2-Reception
switchport mode access
spanning-tree portfast
no shutdown
exit

interface FastEthernet0/4
description PC3-Sales
switchport mode access
spanning-tree portfast
no shutdown
exit

interface FastEthernet0/5
description PC4-Sales
switchport mode access
spanning-tree portfast
no shutdown
exit

interface FastEthernet0/6
description PC5-Manager
switchport mode access
spanning-tree portfast
no shutdown
exit

end

copy running-config startup-config