# STATIC ROUTING

A network administrator manually configures the path.

# Example: 
You tell R1 to reach 192.168.3.0/24 through 10.0.0.2.

Cisco command:

ip route 192.168.3.0 255.255.255.0 10.0.0.2

# NOTES:
    The default gateway configuration is also called a default route.
    It is a route to 0.0.0.0/0 = all netmask bits are set to 0. Includes all addresses from 0.0.0.0 to 255.255.255.255

# Suppose a PC connected to R1 wants to communicate with a PC in R3's LAN.

R1 knows its directly connected networks, but it might not know how to reach 192.168.3.0/24.

You can manually configure a route on R1 telling it to send traffic for that network towards R2.

Key idea: Static routes are configured manually; dynamic routing protocols, such as OSPF, learn routes automatically from other routers.
# 2. The static route command
This is one of the most important commands to know for the CCNA:

# Router(config)# ip route destination-network subnet-mask next-hop-ip

There are three main parts:
# Part	              Meaning
destination-network	  The network you want to reach
subnet-mask	          The mask of that destination network
next-hop-ip	          The IP address of the next router along the path

# R1(config)# ip route 192.168.3.0 255.255.255.0 10.0.12.2



# DATA TRAVELING VIA DIFFERENT HOPS(ROUTERS)
![alt text](image-6.png)

Here’s a clearer diagram showing the full journey from PC1 to PC4, with the MAC addresses at every hop.

# The most important part to remember:
- PC1 → R1: Destination MAC = R1’s G0/0 interface MAC.
- R1 → R2: Destination MAC = R2’s receiving interface MAC.
- R2 → R3: Destination MAC = R3’s receiving interface MAC.
- R3 → PC4: Destination MAC = PC4’s MAC address.

# Remember: 
Every router-to-router link uses the MAC addresses of the two interfaces connected to that link. The IP addresses stay the same throughout normal routing, but the MAC addresses change at each hop.


