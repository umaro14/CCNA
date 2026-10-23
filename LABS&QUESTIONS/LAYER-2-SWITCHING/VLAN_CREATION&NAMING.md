# VLAN CREATING AND NAMING

Switch>en
% Privilege EXEC capabilities activated.
Switch#conf t
Enter configuration commands, one per line. End with CNTL/Z.
Switch(config)#vlan 10
% VLAN 10 created/accessed, database register succeeded.
Switch(config-vlan)#name sales
% VLAN 10 renamed to: sales
Switch(config-vlan)#vlan 20
% VLAN 20 created/accessed, database register succeeded.
Switch(config-vlan)#name Engineering
% VLAN 20 renamed to: Engineering
Switch(config-vlan)#

# What is the point of this lab ?

Create VLANs such as VLAN 10 (Sales) and VLAN 20 (Engineering)
Assign switch ports to the correct VLANs
Separate devices into different broadcast domains
Understand how VLANs help optimize network traffic and reduce broadcast traffic
Prevent unnecessary Layer 2 broadcast propagation between departments

# For the Cisco interview tip, you could say:

“VLANs divide a Layer 2 network into separate broadcast domains, which reduces broadcast traffic and improves network performance and segmentation.”

provides network operators the blueprint to maintain oeack performance, secure assets, and configure dynamic loops.


# REFERENCES 
# enable
    Unlocks "Privileged EXEC Mode", which is Cisco's version of running as "Administrator" or "root". This gives you elevated access to view system diagnostics and prepare to configure the hardware.

# configure terminal
    Enters "Global Configuration Mode". Think of this as opening the device's global "Control Panel". Any settings configured here carry immediate effect across the network card components.

# vlan 10
    Constructs a Virtual Local Area Network (VLAN) partition #10 in the switch database. This isolates broadcast domains to keep different users safely segmented.

# name Sales
    Assigns the descriptive label "Sales" to your custom VLAN, making it extremely clear for human network engineers to identify the department owning this virtual subnet.