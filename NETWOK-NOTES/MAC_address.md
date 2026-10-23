# MAC Address

A MAC address (Media Access Control address) is a unique identifier assigned to a network interface such as an Ethernet or Wi-Fi adapter.

It is 48 bits (6 bytes) long and is normally written as 12 hexadecimal digits.

It operates mainly at Layer 2 (Data Link Layer) of the OSI model.

A switch learns these MAC addresses and uses its MAC address table to decide which port should receive the frame.


# Important CCNA point

Think of it like this:

# IP address → "Where is the device?"
# MAC address → "Which network interface is it?"

# ARP (Address Resolution peotocol)
   is what allows a device to discover the MAC address associated with an IPv4 address.

| MAC Address                             | IP Address                                       |
| --------------------------------------- | ------------------------------------------------ |
| Layer 2                                 | Layer 3                                          |
| Identifies a network interface          | Identifies a device/interface's network location |
| Usually assigned to the network adapter | Can be assigned dynamically or manually          |
| Used inside the local network           | Used for communication between networks          |
| Example: `00:1A:2B:3C:4D:5E`            | Example: `192.168.1.10`                          |


# 1. Unicast Frame

#    Unicast = one sender → one specific receiver

    The switch knows the destination MAC address and sends the frame only out the port where that device is located.

    PC1 ─── Switch ─── PC2
    PC1 sends:
        Source MAC:      PC1
        Destination MAC: PC2
    
    If the switch knows PC2 is on Fa0/2, it forwards the frame only through Fa0/2.


# 2. Unknown Unicast Frame

#   Unknown unicast = one sender → one receiver, but the switch doesn't know where the destination is.

Example:

PC1 ─── Switch ───--- PC2
                  ├── PC3
                  └── PC4

PC1 sends a frame to PC2, but the switch doesn't have PC2's MAC address in its MAC address table.

The switch floods the frame out all ports in the same VLAN except the port it came from.

             ┌── PC2 ✅
PC1 → Switch ├── PC3
             └── PC4

PC2 receives it and responds.

The switch can then learn PC2's MAC address from the response.

# Important

An unknown unicast is NOT the same as a broadcast.

The destination is still one specific device; the switch simply doesn't know which port leads to it.


# 3. Broadcast

Broadcast = one sender → everyone in the broadcast domain.

The destination MAC address is: FF:FF:FF:FF:FF:FF

The switch floods the frame to all ports in the same VLAN, except the port it arrived on.

             ┌── PC2
             ├── PC3
PC1 → Switch ├── PC4
             └── PC5

Everyone receives it.
# Example: ARP Request

When PC1 asks:

"Who has 192.168.1.20?"

It sends an ARP Request as a broadcast.

Think: 🧑 → 👨‍👩‍👧‍👦

# 4. Multicast

Multicast = one sender → a specific group of receivers.

Not everyone needs to receive the frame.

Example:

             ┌── PC2 ✅
             ├── PC3 ❌
PC1 → Switch ├── PC4 ✅
             └── PC5 ❌

PC2 and PC4 are members of the multicast group, so they receive the traffic.

Multicast MAC addresses use the range: 01:00:5E:xx:xx:xx
Multicast is commonly used for things like streaming, routing protocols, and group-based network communication.

Think: 🧑 → 👥

# The key Difference:
| Type                | Sender → Receiver | What does switch do?            |
| ------------------- | ----------------- | ------------------------------- |
| **Unicast**         | One → One         | Sends to known destination port |
| **Unknown Unicast** | One → One         | Floods within the VLAN          |
| **Broadcast**       | One → Everyone    | Floods within the VLAN          |
| **Multicast**       | One → Group       | Sends to interested group/ports |


A switch floods unknown unicast and broadcast traffic within the VLAN, but a router does not normally forward Layer-2 broadcasts between networks.