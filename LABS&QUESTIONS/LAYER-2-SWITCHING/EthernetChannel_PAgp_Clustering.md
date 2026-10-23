# What is PAgP?

# PAgP = Port Aggregation Protocol

It is a Cisco proprietary protocol used to automatically negotiate an EtherChannel between two switches.

Think of it like this:

SW1                         SW2
 |                           |
 |--- Cable 1 ---------------|
 |--- Cable 2 ---------------|
 |--- Cable 3 ---------------|

Normally, STP might see these as three separate links and block some of them to prevent a loop.

With EtherChannel:

SW1                         SW2
 |                           |
 |===========================|
       EtherChannel
       = 1 logical link

STP sees the three physical cables as ONE logical link.

# 🟢 Where does PAgP come in?

PAgP helps the two switches agree to form the EtherChannel.

For example:
SW1                         SW2
                    PAgP
      "Should we       ↔      "Yes, let's
       form an               form one!"
      EtherChannel?"
    
Once they agree, the physical links are bundled together.

PAgP modes

There are two important PAgP modes:

# Mode	     Meaning
desirable	 Actively tries to form EtherChannel
auto	     Waits for the other switch to ask.

# ⚠️ PAgP vs LACP

You will see two main negotiation protocols for EtherChannel:

PAgP

Cisco proprietary
desirable
auto

LACP

Open standard (IEEE 802.3ad/802.1AX)
active
passive

So when you previously saw:

channel-group 1 mode active

that was LACP, not PAgP.

And:

channel-group 1 mode desirable

is PAgP.

🧠 Easy way to remember

PAgP = Cisco's EtherChannel negotiation

LACP = standard EtherChannel negotiation

Both are used to help create the same thing:

multiple physical cables → one logical EtherChannel.




# If you're deciding between PAgP and LACP, use this simple rule:

🟢 Choose LACP in most situations

LACP is the modern, standards-based choice.

LACP
active ↔ active
active ↔ passive

For example:

SW1                         SW2
LACP active                 LACP active
   |                           |
   |--- cable -----------------|
   |--- cable -----------------|
   |--- cable -----------------|

The switches negotiate and create the EtherChannel.

🔵 Choose PAgP when...

Use PAgP mainly when you're working with Cisco-only equipment and the environment specifically uses PAgP.

PAgP
desirable ↔ desirable
desirable ↔ auto

For example:

SW1                         SW2
PAgP desirable              PAgP auto

They can form the EtherChannel.

🚫 Important

You cannot mix PAgP and LACP:

PAgP desirable  ❌  LACP active

They are different negotiation protocols.

🧠 For your CCNA

I'd remember it like this:

Protocol	Modes	Think
LACP	active / passive	Standard / preferred
PAgP	desirable / auto	Cisco proprietary
Static	on	No negotiation

So if a CCNA question asks which EtherChannel protocol should you use, LACP is usually the protocol you should think of first.


# Script 
enable
configure terminal
interface GigabitEthernet0/1
channel-group 1 mode desirable