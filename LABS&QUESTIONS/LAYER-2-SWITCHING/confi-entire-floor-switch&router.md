# SCRIPT

# enable
# configure terminal
# hostname Floor3-GW
# interface GigabitEthernet0/0
# no shutdown
# exit
# interface GigabitEthernet0/0.10
# encapsulation dot1Q 10
# ip address 10.3.10.1 255.255.255.0
# no shutdown
# exit
# interface GigabitEthernet0/0.20
# encapsulation dot1Q 20
# ip address 10.3.20.1 255.255.255.0
# no shutdown
# exit
# interface GigabitEthernet0/0.30
# encapsulation dot1Q 30
# ip address 10.3.30.1 255.255.255.0
# no shutdown
# exit
# interface GigabitEthernet0/0.99
# encapsulation dot1Q 99
# ip address 10.3.99.1 255.255.255.0
# no shutdown
# exit
# hostname Floor3-SW1
# vlan 10
# name Sales
# exit
# vlan 20
# name Engineering
# exit
# vlan 30
# name Voice
# exit
# vlan 99
# name Management
# exit
# interface FastEthernet0/1
# switchport mode access
# switchport access vlan 10
# switchport port-security
# switchport port-security mac-address sticky
# no shutdown
# exit
# interface FastEthernet0/2
# switchport mode access
# switchport access vlan 10
# switchport port-security
# switchport port-security mac-address sticky
# no shutdown
# exit
# interface FastEthernet0/3
# switchport mode access
# switchport access vlan 20
# switchport port-security
# switchport port-security mac-address sticky
# no shutdown
# exit
# interface FastEthernet0/4
# switchport mode access
# switchport access vlan 20
# switchport port-security
# switchport port-security mac-address sticky
# no shutdown
# exit
# interface FastEthernet0/5
# switchport mode access
# switchport access vlan 30
# switchport port-security
# no shutdown
# exit
# interface FastEthernet0/6
# switchport mode access
# switchport access vlan 30
# switchport port-security
# no shutdown
# exit
# interface GigabitEthernet0/1
# switchport mode trunk
# switchport trunk native vlan 99
# no shutdown
# exit
# interface Vlan99
# ip address 10.3.99.2 255.255.255.0
# no shutdown
# exit
# ip default-gateway 10.3.99.1