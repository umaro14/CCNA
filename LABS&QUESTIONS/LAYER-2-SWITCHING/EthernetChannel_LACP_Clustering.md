# channel-group 1 mode active

This command is used when you want to combine multiple cables between switches into one logical connection.

# LACP stands for:

#   Link Aggregation Control Protocol  #

Break it down:

Link → network connection/cable
Aggregation → combining multiple links together
Control Protocol → a protocol that helps the switches negotiate and manage the connection

# Imagine you have:
SW1                    SW2

F0/1 ================= F0/1
F0/2 ================= F0/2

You have two cables connecting the switches.
Normally, the switches see these as two separate links.

# With EtherChannel, you can combine them:
SW1                    SW2

F0/1 =====\        /=== F0/1
           \      /
            ======
           ONE
          LOGICAL
           LINK
            ======
           /      \
F0/2 =====/        \=== F0/2

Now the switches treat the two physical cables as one logical link.

# Why use Two cables ?
Because one cable may not be enough between two switches.

There are two main reasons for having multiple cables:

# 1. More bandwidth

Imagine:

SW1 ================= SW2
          1 Gbps

That link can carry up to 1 Gbps.

If you have two links:

SW1 ================= SW2
          1 Gbps

SW1 ================= SW2
          1 Gbps

You have 2 × 1-Gbps physical links available.

# 2. Redundancy

Suppose you only have one cable:

SW1 ================= SW2
             ❌

Cable breaks → SW1 and SW2 lose their connection.

With two cables:

SW1 ================= SW2
              ✓

SW1 ================= SW2
              ❌

One cable fails, but the other can continue carrying traffic.

# But there's a problem without EtherChannel

If you simply connect two switches with two cables:

SW1 ================= SW2
      \             /
       ============ 

You've created two Layer 2 paths.

# That can create a network loop.

That's where STP (Spanning Tree Protocol) comes in. STP may block one of the links:

SW1 ================= SW2
       ACTIVE ✓

SW1 ---------X-------- SW2
       BLOCKED

So you're not getting the full benefit of having both cables.

# EtherChannel solves this

EtherChannel tells the switches:

"These multiple physical cables are actually one logical link."

SW1                    SW2
 |                       |
 |====== EtherChannel ===|
 |                       |

Now you get bandwidth + redundancy, while STP sees the EtherChannel as one logical connection.

🧠 Think of it like a road

One cable:

🚗 🚗 🚗 →  ONE LANE → 🚗 🚗 🚗

Two cables:

🚗 🚗 → LANE 1 →
🚗 🚗 → LANE 2 →

EtherChannel is basically saying:

"Treat these lanes as one larger road."

That's why network engineers might deliberately use two, four, or more cables between important switches.


channel-group 1 mode active
             ↑
          group ID

The 1 is simply the number/name of the EtherChannel group.

# Script
enable
configure terminal
interface GigabitEthernet0/1
channel-group 1 mode active

# --------------------------------------

Switch-Core>
en
% Privilege EXEC capabilities activated.
Switch-Core#
conf t
Enter configuration commands, one per line. End with CNTL/Z.
Switch-Core(config)#
int G0/1
% Entered configuration interface parameters for GigabitEthernet0/1.
Switch-Core(config-if)#
channel-group 1 mode active
% Channel aggregate group assigned. Enforcing channel properties.
Switch-Core(config-if)#
