A switch uses the source MAC address of an incoming Ethernet frame to learn and populate its MAC address table.

Example

# Imagine:

    PC1                  SW1                  PC2

| | |
| Ethernet Frame | |
|------------------->| |
| Source MAC: AAAA | |

When SW1 receives the frame, it looks at:

Source MAC: AAAA.AAAA.AAAA
Incoming port: Fa0/1

It learns:

## MAC Address Port

AAAA.AAAA.AAAA Fa0/1

This is called MAC address learning.

Then what about the destination MAC?

The switch uses the destination MAC address to decide where to forward the frame.

So remember:

🟢 Source MAC → Learn → MAC address table
🔵 Destination MAC → Look up → Forward frame

Example

PC1 sends a frame to PC2:

Source MAC: AAAA.AAAA.AAAA
Destination MAC: BBBB.BBBB.BBBB

SW1:

# Looks at source MAC → learns AAAA is on Fa0/1.
# Looks at destination MAC → searches MAC table for BBBB.
# If BBBB is known → forwards only through the correct port.
# If BBBB is unknown → unknown unicast flooding (except the incoming port).

That's one of the fundamental concepts of Ethernet switching, and you've got it right.

# The preamble is a sequence of bits at the very beginning of an Ethernet frame that helps the receiving network device synchronise its clock with the incoming signal.

Think of it like:

📢 "Get ready — an Ethernet frame is about to arrive."

Ethernet frame structure

# Simplified:

┌───────────┬──────────────┬──────────────┬─────────────┬─────────┐
│ Preamble │ Dest. MAC │ Source MAC │ Type/Length │ Data... │
│ 7 bytes │ 6 bytes │ 6 bytes │ 2 bytes │ │
└───────────┴──────────────┴──────────────┴─────────────┴─────────┘

                                              ... FCS at the end

The preamble is 7 bytes (56 bits).

After it comes the Start Frame Delimiter (SFD), which is 1 byte (8 bits).

So you can remember:

Preamble → SFD → Destination MAC → Source MAC → ...
7 B 1 B
🧠 What does it actually do?

Ethernet devices need to know when the actual frame begins and synchronise to the incoming bit stream.

The preamble consists of a repeating pattern:

10101010 10101010 10101010 ...

The receiver detects this pattern and synchronises itself before the actual Ethernet frame arrives.

# Ethernet Frame Format

An Ethernet frame is structured like this:
┌────────────┬───────────────┬───────────────┬──────────────┬──────────────┬─────────┐
│ Preamble │ Destination │ Source │ Type/Length │ Data │ FCS │
│ 7 bytes │ MAC 6 B │ MAC 6 B │ 2 B │ 46–1500 B │ 4 B │
└────────────┴───────────────┴───────────────┴──────────────┴──────────────┴─────────┘

Preamble → SFD → Destination MAC → Source MAC → Type/Length → Data → FCS
7 B 1 B 6 B 6 B 2 B 46–1500 B 4 B

# 🖧 Ethernet Frame Format

An Ethernet frame is the package of information that a device sends across an Ethernet network.

Think of it like a parcel 📦.

The data you want to send is placed inside a structure containing information that tells the network:

# Who sent it?

# Who should receive it?

# What type of data is inside?

# Is the data valid/corrupted?

# 1️⃣ Preamble

Size: 7 bytes

The preamble comes at the beginning.

Preamble
7 bytes

Its job is to help the receiving device synchronise its clock with the incoming Ethernet signal.

Think:

"Get ready, a frame is coming."

It consists of a repeating pattern of bits:

10101010 10101010 10101010 ...

# 2️⃣ SFD — Start Frame Delimiter

Size: 1 byte

SFD stands for:

Start Frame Delimiter

It tells the receiving device:

"The actual Ethernet frame starts now."

So:

Preamble → SFD → Ethernet Frame

Think:

# Preamble = Get ready

# SFD = START!

Important

# NOTE: The preamble + SFD are normally not counted as part of the 64–1518 byte Ethernet frame size.

# 3️⃣ Destination MAC Address

Size: 6 bytes

This tells the network where the frame is going.

Example:

Destination MAC:
00:1A:2B:3C:4D:5E

A switch looks at this address to decide where to forward the frame.

For example:

             Switch
           /    |    \
         PC1   PC2   PC3

If PC1 sends to PC3:

Source MAC: PC1
Destination MAC: PC3

The switch looks at the destination MAC and forwards the frame toward PC3.

# 4️⃣ Source MAC Address

Size: 6 bytes

This identifies the device that sent the frame.

Example:

Source MAC:
AA:AA:AA:AA:AA:AA

This is extremely important for switches.

Remember what you just learned:

🟢 Switches learn MAC addresses from the SOURCE MAC address.

For example, PC1 sends a frame into SW1 through Fa0/1:

Source MAC = PC1
Incoming port = Fa0/1

The switch learns:

## MAC Address Port

AAAA.AAAA.AAAA Fa0/1

So:

Source MAC → Learning

Destination MAC → Forwarding

# 5️⃣ Type/Length

Size: 2 bytes

This field tells the receiving device what is contained inside the Ethernet payload.

In modern Ethernet, it is commonly an EtherType.

Examples:

0x0800 → IPv4
0x86DD → IPv6
0x0806 → ARP

So if the Type field says:

0x0800

the device knows:

"The payload contains an IPv4 packet."

6️⃣ Data / Payload

Size: 46–1500 bytes

This is where the actual information being transported is located.

For example:

Ethernet Frame
│
└── Payload
│
└── IPv4 Packet
│
└── TCP Segment
│
└── Application Data

This is where encapsulation becomes important.

For example:

Application Data
↓
TCP Segment
↓
IP Packet
↓
Ethernet Frame

# 📏 Ethernet frame size

For a standard Ethernet frame:

Minimum = 64 bytes

Maximum = 1518 bytes

This calculation does NOT include the 7-byte preamble or 1-byte SFD.

# PADDING BYTES:

If the payload is less than 46 bytes, Ethernet adds padding to bring the payload up to 46 bytes.

Example

Suppose a device only has 20 bytes of data to send:

Payload = 20 bytes
Required minimum = 46 bytes

Padding = 46 - 20
= 26 bytes

So the Ethernet frame becomes:

Destination MAC 6 bytes
Source MAC 6 bytes
Type/Length 2 bytes
Payload 20 bytes
Padding 26 bytes
FCS 4 bytes
─────
64 bytes
🧠 Why?

Ethernet has a minimum frame size of 64 bytes (not counting the preamble/SFD).

So:

Payload < 46 bytes → Ethernet adds padding → payload + padding = 46 bytes

The receiving device knows how to distinguish the actual higher-layer data from the padding based on the protocol information/length.
