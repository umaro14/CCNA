# STP

A Spanning Tree is a way for switches to prevent loops in a Layer 2 network.

Let's make it very simple.

🔴 The problem: switch loops

Imagine three switches connected in a triangle:

        SW1
       /   \
      /     \
    SW2-----SW3

This is redundant because there are multiple paths between the switches.

That's good for backup, but it creates a problem: Layer 2 loops.

A broadcast could keep travelling around:

SW1 → SW2 → SW3 → SW1 → SW2 → SW3 → ...

It can continue indefinitely.
This can cause a broadcast storm and other network problems.

# 🌳 What does Spanning Tree do?
STP (Spanning Tree Protocol) detects these redundant paths and blocks some ports to prevent the loop.

For example:

        SW1
       /   \
      /     \
    SW2-----SW3
           🚫

One link is placed into a blocking state.

Now there is still a backup path, but the loop is prevented:
        SW1
       /   \
      /     \
    SW2     SW3

# If a link fails:
        SW1
       /   \
      /     \
    SW2-----SW3

STP can unblock the previously blocked path.

# 🧠 The easiest way to remember it

# STP = prevents Layer 2 loops while keeping redundant links available.

And Spanning Tree is basically the loop-free logical path that STP creates through the network.

That's why it's called a tree 🌳 — a tree structure has paths that don't form loops.

# Script 
enable
configure terminal
spanning-tree mode pvst
show spanning-tree

# spanning-tree mode pvst

This is a Cisco switch command that tells the switch:

"Use PVST (Per-VLAN Spanning Tree) as the Spanning Tree mode."

Break it down:

spanning-tree → enables/configures Spanning Tree Protocol.
mode → tells the switch which STP mode to use.
pvst → Per-VLAN Spanning Tree.
🧠 What does PVST do?

Suppose your switch has:

VLAN 10
VLAN 20
VLAN 30

PVST runs a separate STP instance for each VLAN:
VLAN 10 → STP
VLAN 20 → STP
VLAN 30 → STP

This means each VLAN can independently decide:

Which switch is the Root Bridge
Which ports should forward
Which ports should block


# Think of PVST as multiple separate trees, one tree per VLAN.

             NETWORK
                │
       ┌────────┼────────┐
       │        │        │
     VLAN 10  VLAN 20  VLAN 30
       🌳       🌳       🌳
      STP      STP      STP

So it's not literally a tree inside another tree, but conceptually you can think of it as:

One network → multiple VLANs → each VLAN has its own spanning-tree topology.

For example:

VLAN 10 → 🌳 its own STP tree
VLAN 20 → 🌳 its own STP tree
VLAN 30 → 🌳 its own STP tree

And each tree can have a different Root Bridge.

# show spanning-tree
Switch-Core(config)#
show spanning-tree
Spanning-tree mode is current configured as: PVST
VLAN0001
  Spanning-tree enabled protocol ieee
  Root ID    Priority    32769
             Address     001a.30bc.ef01
             This bridge is the root
Switch-Core(config)#
