# 01-Basic-Office-Network

Project documentation.

# Project 01 — Basic Office Network

## Overview

This project demonstrates the design and configuration of a small office IPv4 network using Cisco Packet Tracer.

The network consists of a Cisco router, a Cisco Layer 2 switch, and five end-user PCs. The router provides DHCP services to automatically assign IP addresses to the PCs.

The project focuses on fundamental networking concepts including IPv4 addressing, DHCP, default gateways, Ethernet switching, MAC address learning, Cisco IOS configuration, and network connectivity testing.

---

## Network Scenario

A small company requires a basic local area network for its employees.

The company has three departments:

- Reception
- Sales
- Management

The network must allow all computers to communicate with each other and access the router.

The router will provide DHCP services so that users do not need to manually configure their IP addresses.

---

## Objectives

The objectives of this project are to:

1. Design a basic office network topology.
2. Configure a Cisco router.
3. Configure a Cisco Layer 2 switch.
4. Configure IPv4 addressing.
5. Configure DHCP on the router.
6. Configure PCs as DHCP clients.
7. Verify Layer 2 MAC address learning.
8. Test connectivity between network devices.
9. Verify the router configuration.
10. Document the network for future troubleshooting.

---

# Network Topology

```text
                         +----------------+
                         |   Office-R1    |
                         | Cisco 2911     |
                         | 192.168.1.1    |
                         +-------+--------+
                                 |
                                 |
                          G0/0   |
                                 |
                         +-------+--------+
                         |   Office-SW1   |
                         | Cisco 2960     |
                         +--+--+--+--+----+
                            |  |  |  |  |
                           PC1 PC2 PC3 PC4 PC5