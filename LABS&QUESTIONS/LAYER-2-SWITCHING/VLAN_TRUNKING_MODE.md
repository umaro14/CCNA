# VLAN TRUNKING MODE

Switch-Core>
en
% Privilege EXEC capabilities activated.
Switch-Core#
conf t
Enter configuration commands, one per line. End with CNTL/Z.
Switch-Core(config)#
interface G0/1
% Entered configuration interface parameters for GigabitEthernet0/1.
Switch-Core(config-if)#
switchport mode trunk
% Interface role changed to: switchport mode trunk
Switch-Core(config-if)#
show ruuning-config
% Unrecognized secondary show argument.
Switch-Core(config-if)#
show runnig-config
% Unrecognized secondary show argument.
Switch-Core(config-if)#
show running-config
Building configuration...
Current configuration : 1854 bytes
!
hostname Switch-Core
!
spanning-tree mode pvst
!
!
interface GigabitEthernet0/1
 shutdown
 switchport mode trunk
!
interface GigabitEthernet0/0
 shutdown
!
!
line con 0
 login local
line vty 0 4
 login local
!
end
Switch-Core(config-if)#


# script
    enable
    configure terminal
    interface GigabitEthernet0/1
    switchport mode trunk
    show running-config

# switchport mode trunk
Instructs the switchport interface to act as a trunk. It converts a single Ethernet line to a virtual multiplexed highway carrying all VLAN traffic via IEEE 802.1Q tags.