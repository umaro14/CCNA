![alt text](image.png)

# Router>
# Router>en
# Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#
Router(config)#
# Router(config)#hostname R1
# R1(config)#show ip interface
            ^
% Invalid input detected at '^' marker.
	
R1(config)#show ip interface brief
            ^
% Invalid input detected at '^' marker.
	
# R1(config)#do show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     unassigned      YES unset  administratively down down 
GigabitEthernet0/1     unassigned      YES unset  administratively down down 
GigabitEthernet0/2     unassigned      YES unset  administratively down down 
Vlan1                  unassigned      YES unset  administratively down down
R1(config)#
R1(config)#
# R1(config)# interface gigabitEthernet 0/0
# R1(config-if)#ip address 15.255.255.254 255.0.0.0
# R1(config-if)#
# R1(config-if)#description ## to SW1 ##
# R1(config-if)#no shutdown

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

# R1(config-if)#
# R1(config-if)#
# R1(config-if)# interface gigabitEthernet 0/1
# R1(config-if)#ip address 182.98.255.254 255.255.0.0
# R1(config-if)#desc ## to SW2 ##
# R1(config-if)#no shutdown

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

# R1(config-if)#
# R1(config-if)#
# R1(config-if)#int g0/2
# R1(config-if)#ip add 201.191.20.254 255.255.255.0
# R1(config-if)#desc ## to SW3 ##
# R1(config-if)#no shut

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/2, changed state to up

R1(config-if)#
# R1(config-if)#end
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#
R1#
# R1#sh ip int br
# Interface              IP-Address      OK? Method Status                Protocol 
# GigabitEthernet0/0     15.255.255.254  YES manual up                    up 
# GigabitEthernet0/1     182.98.255.254  YES manual up                    up 
# GigabitEthernet0/2     201.191.20.254  YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
R1#



##### Configuring Interfaces With Duplex and  Speed #####
R1(config)#int G0/0
R1(config-if)#do show ip int br
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     unassigned      YES unset  administratively down down 
GigabitEthernet0/1     unassigned      YES unset  administratively down down 
GigabitEthernet0/2     unassigned      YES unset  administratively down down 
Vlan1                  unassigned      YES unset  administratively down down
R1(config-if)#
R1(config-if)#
R1(config-if)#ip address 172.16.255.254 255.255.0.0
R1(config-if)#speed 1000
R1(config-if)#duplex full
R1(config-if)#desc ## to SW1 ##
R1(config-if)#no shut


### By default in packet tracer the protocol is down unlike in real Cisco Router device ###
R1(config-if)#do sh ip int br
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     172.16.255.254  YES manual up                    down 
GigabitEthernet0/1     unassigned      YES unset  administratively down down 
GigabitEthernet0/2     unassigned      YES unset  administratively down down 
Vlan1                  unassigned      YES unset  administratively down down

### here we use range to describe interfaces at once instead of one by one ###
R1(config-if)#
R1(config-if)#
R1(config-if)#int range G0/1 - 2
R1(config-if-range)#desc ## not in use ##
R1(config-if-range)#do show run


# Why not use a Router instead of a Switch for all LAN ports?
1. Subnet Fragmentation: Every router interface requires its own unique IP subnet, making local host communication complex.

2. High Cost: Dedicated router ports with specialized processing hardware are vastly more expensive per-port than switch ports.

3. Expansion Simplicity: Switches allow effortless, plug-and-play scaling of hosts within the same broadcast domain.
![alt text](image-3.png)
![alt text](image-4.png)
![alt text](image-5.png)
![alt text](image-6.png)
![alt text](image-7.png)

# ⚖️5. NGFW vs. NGIPS Comparison (Official Exam Focus)
Both modern Next-Generation Firewalls and Next-Generation Intrusion Prevention Systems are deployed in enterprise security architectures, but fulfill complementary responsibilities:
![alt text](image-8.png)