# pc1 wants to reach pc4 using static routing

Path chosen: PC1 → R1 → R3 → R4 → PC4 (the top route). R2 could carry the traffic instead, but this example doesn’t use it.

Ideas worth remembering

Hosts only know their gateway. PC1 can’t see past its own subnet. Anything outside 192.168.1.0/24 goes to 192.168.1.1, which is R1’s G0/2 interface.
Routers only know their own interfaces. R1 starts out knowing 192.168.1.0, 12.0 and 13.0, but not 192.168.4.0/24. Without a static route it drops the packet.
A static route says “for this network, hand the packet to that next hop.” The next hop has to be on a network the router is directly connected to.
The IP addresses stay the same the whole way (192.168.1.10 → 192.168.4.10). At every hop the MAC addresses are replaced with new ones and the TTL goes down by 1.
Routing has to work in both directions. The ping only succeeds if the reply can also get back to 192.168.1.0/24. Forgetting the return routes is the most common mistake in this lab.

# Configuration needed

! PC1 default gateway: 192.168.1.1   
PC4 default gateway: 192.168.4.4

R1(config)# ip route 192.168.4.0 255.255.255.0 192.168.13.3 (R3 ip)
R3(config)# ip route 192.168.4.0 255.255.255.0 192.168.34.4

R3(config)# ip route 192.168.1.0 255.255.255.0 192.168.13.1
R4(config)# ip route 192.168.1.0 255.255.255.0 192.168.34.3

# FORMAT
ip route  192.168.4.0   255.255.255.0   192.168.13.3
          └ network ┘   └── mask ───┘   └ next hop ┘

# It means: 
“To reach any address in 192.168.4.0/24, send the packet to 192.168.13.3.”

# Network + mask: 
    the destination subnet. The mask 255.255.255.0 is the same as /24, so this covers 192.168.4.0 to 192.168.4.255, which includes PC4 (192.168.4.10).
# Next hop: 
    the IP address of the next router on the path. This router must be on a network the current router is directly connected to, so it can reach the next hop in one step.

# NOTE:
A static route only tells a router the next step, not the whole path. Each router on the way needs its own route to the destination.

The default gateways
PC1 default gateway: 192.168.1.1
PC4 default gateway: 192.168.4.4

# Why there are four routes?
Every router already knows its directly connected networks. A static route is only needed for networks a router can’t see:

# Router	  Knows (connected)	  Missing on this path	    Static routes needed
R1	          1.0, 12.0, 13.0	       4.0	                       1
R3	          13.0, 34.0	      1.0 and 4.0	                   2
R4	          4.0, 24.0, 34.0	      1.0	                       1

# NOTE:
R3 is in the middle and isn’t connected to either PC network, so it needs routes in both directions. R1 and R4 each sit next to one PC network and only need a route to the network at the far end.

# Why the return routes matter?
If you configure only the “going there” routes, PC1’s ping does reach PC4. PC4 then sends a reply to 192.168.1.10, and R4 has no route to 192.168.1.0/24, so it drops the reply. PC1 sees Request timed out even though its packet arrived. A ping only succeeds when routes exist in both directions.


# Note:
R1 doesn’t need a route back to 192.168.1.0/24, and R4 doesn’t need one to 192.168.4.0/24. Each router is directly connected to the network on its own side

Below is the full configuration for R1, R3 and R4, including interface addressing so the static routes have connected networks to point to.

# 1. Full configuration
# R1

enable
configure terminal
hostname R1
!
interface GigabitEthernet0/0
 description Link to R3
 ip address 192.168.13.1 255.255.255.0
 no shutdown
!
interface GigabitEthernet0/1
 description Link to R2
 ip address 192.168.12.1 255.255.255.0
 no shutdown
!
interface GigabitEthernet0/2
 description PC1 LAN
 ip address 192.168.1.1 255.255.255.0
 no shutdown
!
ip route 192.168.4.0 255.255.255.0 192.168.13.3
end
write memory

# R3

enable
configure terminal
hostname R3
!
interface GigabitEthernet0/0
 description Link to R1
 ip address 192.168.13.3 255.255.255.0
 no shutdown
!
interface GigabitEthernet0/1
 description Link to R4
 ip address 192.168.34.3 255.255.255.0
 no shutdown
!
ip route 192.168.4.0 255.255.255.0 192.168.34.4
ip route 192.168.1.0 255.255.255.0 192.168.13.1
end
write memory

# R4

enable
configure terminal
hostname R4
!
interface GigabitEthernet0/0
 description Link to R2
 ip address 192.168.24.4 255.255.255.0
 no shutdown
!
interface GigabitEthernet0/1
 description Link to R3
 ip address 192.168.34.4 255.255.255.0
 no shutdown
!
interface GigabitEthernet0/2
 description PC4 LAN
 ip address 192.168.4.4 255.255.255.0
 no shutdown
!
ip route 192.168.1.0 255.255.255.0 192.168.34.3
end
write memory



# PCs

PC1: IP 192.168.1.10, mask 255.255.255.0, gateway 192.168.1.1
PC4: IP 192.168.4.10, mask 255.255.255.0, gateway 192.168.4.4
R2 needs no static routes for this path. Configure its interfaces only if you want the 12.0 and 24.0 links up.

2. Verify with show ip route
On real IOS the full table includes the code legend at the top. The parts that matter are shown below. If you only want static routes, run show ip route static.

R1

R3: there should be two S routes.

R4

How to read these lines:

S means a static route and C means a connected network.
L is the router’s own interface address, shown as a /32.
[1/0] means administrative distance 1 (the default for static routes) and metric 0.
If a static route is missing from the table, its next hop is probably unreachable. That usually means the exit interface is down or has the wrong IP.
Also check the interfaces:

Each interface you use should show up / up.

3. Verify with ping
Test one hop at a time so you can see exactly where it breaks.

#	From	Command	Proves
1	PC1	ping 192.168.1.1	PC1 can reach its gateway
2	R1	ping 192.168.13.3	The R1 ↔ R3 link works
3	R3	ping 192.168.34.4	The R3 ↔ R4 link works
4	R1	ping 192.168.4.10 source g0/2	Routes work in both directions without the PCs
5	PC1	ping 192.168.4.10	End-to-end test
6	PC1	tracert 192.168.4.10 (Windows) or traceroute	The path goes through R1, R3 and R4