# Switchport Port Security
Switchport Port Security is a Cisco switch feature that controls which devices are allowed to connect to a switch port.    

# Think of it like a security guard at each switch port:

🔌 “Only this device is allowed to use this port. If another device appears, take action.”

Simple example

Imagine:

        SW1
         |
      F0/1
         |
       PC1

You configure port security on F0/1 and tell the switch:

“Only PC1's MAC address is allowed here.”

# If someone disconnects PC1 and connects another laptop:
        SW1
         |
      F0/1
         |
     ❌ Laptop

# Why MAC addresses?

Every network device has a MAC address, for example:
# PC1 → 00:1A:2B:3C:4D:5E
Port security can use that MAC address to identify the permitted device.

# A basic configuration looks like:
SW1(config)# interface fastEthernet 0/1
SW1(config-if)# switchport mode access
SW1(config-if)# switchport port-security

# Then you can specify how many MAC addresses are allowed:
SW1(config-if)# switchport port-security maximum 1

# And specify the allowed MAC address:

SW1(config-if)# switchport port-security mac-address 001A.2B3C.4D5E

# What happens if an unauthorized device connects?

You configure a violation mode.

The main ones you should know for CCNA are:

# Mode	    What happens
protect	    Drops frames from unauthorized MAC addresses
restrict	Drops frames + records/logs the violation
shutdown	Puts the port into an error-disabled state

Shutdown is the default violation mode.

So the basic idea is:

Port Security
     ↓
Checks MAC address
     ↓
Is it allowed?
   ↙       ↘
 YES        NO
 ↓          ↓
Allow     Violation action
traffic   (protect/restrict/shutdown)

# One important CCNA point

Port security is normally used on access ports, where end devices such as PCs, printers, or IP phones connect.

# 1. switchport port-security

This turns on port security for the interface.

SW1(config-if)# switchport port-security

Think of it as:

🔒 “Start checking which MAC addresses are allowed on this port.”

Without this command, port security is not enabled.

# 2. switchport port-security mac-address sticky

This tells the switch:

📌 “Learn the MAC address of the device connected to this port and automatically make it an allowed MAC address.”

Example:

SW1(config)# interface f0/1
SW1(config-if)# switchport mode access
SW1(config-if)# switchport port-security
SW1(config-if)# switchport port-security mac-address sticky

Suppose PC1 is connected:

        SW1
         |
       F0/1
         |
        PC1
   MAC: AAAA.BBBB.CCCC

The switch sees PC1's MAC address and learns it:

F0/1 → AAAA.BBBB.CCCC

That MAC address becomes a secure MAC address.

If someone disconnects PC1 and connects another PC:

        SW1
         |
       F0/1
         |
        ❌ PC2
   MAC: 1111.2222.3333


   # CCNA tip: 
        sticky is useful because you don't have to manually type the PC's MAC address. The switch learns it for you.
    

# If you don't use sticky, you can manually specify which MAC address is allowed:

 # SW1(config-if)# switchport port-security
 # SW1(config-if)# switchport port-security mac-address 001A.2B3C.4D5E

You're basically telling the switch:

# 🔒 “Only allow the device with MAC address 001A.2B3C.4D5E on this port.”

Compare the two methods

Manual MAC address:

switchport port-security
switchport port-security mac-address 001A.2B3C.4D5E

You know the MAC → you type it manually.

Sticky:

switchport port-security
switchport port-security mac-address sticky

The switch learns the MAC automatically.

# Yes. 👍 You can allow more than one MAC address on a single switch port.

You use:

# switchport port-security maximum 2

This tells the switch:

“Allow up to 2 secure MAC addresses on this port.”

Example
SW1(config-if)# switchport mode access
SW1(config-if)# switchport port-security
SW1(config-if)# switchport port-security maximum 2

Then you could manually specify two MAC addresses:

SW1(config-if)# switchport port-security mac-address 001A.2B3C.4D5E
SW1(config-if)# switchport port-security mac-address 0066.7788.99AA

Now:

F0/1
 │
 ├── MAC 001A.2B3C.4D5E ✅
 └── MAC 0066.7788.99AA ✅

A third different MAC address would trigger a port-security violation.

With sticky

You can also let the switch learn multiple devices:

switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky

# script
enable
configure terminal
interface GigabitEthernet0/1
switchport port-security
switchport port-security mac-address sticky