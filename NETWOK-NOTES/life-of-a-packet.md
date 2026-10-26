# Life of a Packet (CCNA)
The key rule: as a packet crosses the network, the IP addresses (Layer 3) stay the same from start to finish. The MAC addresses (Layer 2) change at every router hop. This assumes no NAT.

PC1 ──── SW1 ──── R1 ════════ R2 ──── SW2 ──── PC2
192.168.1.10   G0/0     G0/1  G0/1    G0/0        192.168.2.10
               .1.1   10.0.0.1 10.0.0.2  .2.1


PC1 has a default gateway of 192.168.1.1. It wants to send data to PC2.

# Step 1: PC1 decides whether the destination is local or remote
PC1 compares its own network with the destination’s network, using its subnet mask (/24):

PC1’s network: 192.168.1.0
PC2’s network: 192.168.2.0

The networks are different, so PC2 is remote. PC1 must send the packet to its default gateway (R1). If PC2 were on the same subnet, PC1 would ARP for PC2 directly.

# Step 2: PC1 ARPs for the gateway’s MAC
PC1 knows R1’s IP but needs R1’s MAC. It checks its ARP cache. If the entry is missing:

- It sends an ARP Request. This is a broadcast to FFFF.FFFF.FFFF asking “Who has 192.168.1.1?”
- SW1 floods the broadcast out every port except the one it came in on. SW1 also learns PC1’s MAC on that incoming port.
- R1 sends an ARP Reply (unicast) with its G0/0 MAC address.

# Step 3: PC1 builds the frame
# Field	      Value
  Src IP	  192.168.1.10 (PC1)
  Dst IP	  192.168.2.10 (PC2)
  Src MAC	  PC1’s MAC
  Dst MAC	  R1 G0/0 MAC ← the next hop, not PC2


# Step 4: SW1 forwards the frame (Layer 2 only)
Learns the source(PC1) MAC and records it against the incoming port in its MAC address table.
# Looks up the destination MAC:
    Known → forwards it out that one port (known unicast) (R1 MAC)
    Unknown → floods it (unknown unicast)
The switch never looks at IP addresses or changes them.

# Step 5: R1 routes the packet (Layer 3)
- Checks the FCS (the frame’s error check). If it’s corrupt, R1 drops the frame.
- Strips the Layer 2 header. The frame was addressed to R1, so that header has done its job.
- Looks up the destination IP 192.168.2.10 in its routing table and uses the longest prefix match. The next hop is 10.0.0.2 out G0/1.
- Decrements the TTL by 1. If the TTL reaches 0, R1 drops the packet and sends back an ICMP Time Exceeded message.
- Recalculates the IP header checksum, because the TTL changed.
- ARPs for the next hop (10.0.0.2) if it doesn’t already have its MAC.
- Re-encapsulates the packet in a new frame:

# Field	          Value
Src IP / Dst IP	  unchanged (PC1 → PC2)
Src MAC	          R1 G0/1 MAC
Dst MAC	          R2 G0/1 MAC