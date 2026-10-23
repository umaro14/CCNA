# STP Root Bridge Election is the process STP uses to decide:

"Which switch will be the main/reference switch for the spanning-tree network?"

Think of the Root Bridge as the starting point of the STP tree 🌳.

1. Imagine several switches
        SW1
       /   \
      /     \
    SW2-----SW3

STP needs to choose one switch as the Root Bridge.

        SW1 👑
       /   \
      /     \
    SW2-----SW3

Now the other switches calculate their best paths toward SW1.


# 2. How does STP choose the Root Bridge?

# Every switch has a Bridge ID.

The Bridge ID is basically:

Bridge ID =
STP Priority + MAC Address

STP chooses the switch with the lowest Bridge ID.

🥇 Priority comes first

For example:

SW1 → Priority 32768
SW2 → Priority 24576
SW3 → Priority 32768

SW2 wins because:

24576 < 32768

So:

        SW2 👑
       /     \
      /       \
    SW1------SW3


# 🧠 Important CCNA point

Lowest priority wins.

# If the priorities are equal:

Lowest MAC address wins.

So remember:

Root Bridge = lowest Bridge ID

And:

Bridge ID = Priority + MAC address

Why do we need a Root Bridge?

Once the Root Bridge is selected, the other switches use it as the reference point for calculating the loop-free paths.

For example:

             ROOT
              SW1
             /   \
            /     \
          SW2-----SW3

STP determines which links should forward and which should block to eliminate the Layer 2 loop.

So the whole process is:

1. Elect Root Bridge → 2. Calculate best paths → 3. Block unnecessary paths → 4. Prevent Layer 2 loops.

# Script
enable
configure terminal
spanning-tree vlan 1 root primary

# 1. spanning-tree

You're configuring Spanning Tree Protocol (STP).

Remember:

STP prevents Layer 2 loops between switches.

# 2. vlan 1

You're telling STP:

"I am talking about VLAN 1."

So this command only affects the STP instance for VLAN 1.

Remember that with PVST, each VLAN has its own STP instance:

VLAN 1  → 🌳 STP
VLAN 10 → 🌳 STP
VLAN 20 → 🌳 STP
3. root primary

This is the important part.

It tells the switch:

"Make me the Root Bridge for VLAN 1."

For example, suppose you have:

SW1
SW2
SW3

Normally, STP elects the Root Bridge automatically based on the lowest Bridge ID.

But if you configure on SW1:

SW1(config)# spanning-tree vlan 1 root primary

you're basically saying:

             SW1 👑
            ROOT
           /    \
         SW2----SW3

SW1 becomes the preferred Root Bridge for VLAN 1.

# 🧠 What does the command actually do?

You might wonder:

# "Does root primary simply force the priority to 0?"

# Not exactly.

Cisco's root primary command automatically adjusts the switch's STP priority so that it becomes the root (or is strongly preferred to become root), rather than making you manually calculate and enter a priority.

That's why it's convenient.

Compare:

Automatic:

spanning-tree vlan 1 root primary

"Make this switch the primary root for VLAN 1."

Manual:

spanning-tree vlan 1 priority 24576

"Set this switch's STP priority to 24576 for VLAN 1."

# ⭐ CCNA memory trick
spanning-tree vlan 1 root primary
             ↓
      VLAN 1 STP
             ↓
       ROOT PRIMARY
             ↓
   "Make this switch
    the Root Bridge"

So the simplest meaning is:

spanning-tree vlan 1 root primary = Make this switch the primary Root Bridge for VLAN 1.