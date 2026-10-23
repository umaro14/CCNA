Rapid Spanning Tree Protocol (RSTP) is basically a faster version of STP.

First, remember normal STP

STP prevents Layer 2 loops:

        SW1
       /   \
      /     \
    SW2-----SW3

STP may block one of the links so frames don't loop.

The downside is that traditional STP can take a while to recover when a link fails.

# ⚡ RSTP

RSTP = Rapid Spanning Tree Protocol

It does the same basic job:

Prevent Layer 2 loops while allowing redundant links.

But it is designed to converge much faster when the network topology changes.

Think:

# STP  → 🐢 slower recovery
# RSTP → ⚡ faster recovery

Cisco terminology

You may see:

spanning-tree mode rapid-pvst

This means:

Rapid PVST+

It combines:

Rapid → faster STP
PVST → separate STP instance for each VLAN

So:

PVST
VLAN 10 → STP
VLAN 20 → STP
VLAN 30 → STP

becomes:

Rapid PVST+
VLAN 10 → Rapid STP
VLAN 20 → Rapid STP
VLAN 30 → Rapid STP

# Script
enable
configure terminal
spanning-tree mode rapid-pvst
show spanning-tree