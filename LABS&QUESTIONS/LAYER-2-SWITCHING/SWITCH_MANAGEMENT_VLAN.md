# A switch management VLAN 
    is a VLAN used specifically to manage the switch itself — for example, connecting to it using SSH, Telnet, or management software.

# Think of it this way:

Normal VLANs = carry user/device traffic
Management VLAN = gives you an IP address to manage the switch

# Why use VLAN 99?

You could technically use VLAN 1, but it's common practice to create a dedicated management VLAN, such as VLAN 99.

For example:

VLAN 10 → Administration
VLAN 20 → Doctors
VLAN 30 → Nurses
VLAN 99 → Management

Then:

                    R1
                     |
                    SW1
          ┌──────────┼──────────┐
          │          │          │
       VLAN 10    VLAN 20    VLAN 30
       Admin      Doctors     Nurses

                     +
                  VLAN 99
                Management
                     |
             192.168.99.10
                  SW1
One important thing

Just configuring:

interface vlan 99
ip address 192.168.99.10 255.255.255.0
no shutdown

doesn't automatically make every port part of VLAN 99.

You also need VLAN 99 to exist:

vlan 99
name MANAGEMENT

And you need a path to VLAN 99, such as a management PC connected to an access port in VLAN 99 or a trunk carrying VLAN 99.

# script
enable
configure terminal
interface Vlan99
ip address 192.168.99.10 255.255.255.0
no shutdown