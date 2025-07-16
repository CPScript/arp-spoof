This is an ARP poisoning attack tool written in C. It performs a man-in-the-middle attack by sending malicious ARP replies to poison the ARP cache of devices on a local network.

**Technical breakdown:**

The program crafts and continuously broadcasts forged ARP reply packets claiming that a specified IP address (typically the gateway) is associated with an attacker-controlled MAC address. This causes other devices on the network to update their ARP tables with incorrect MAC-to-IP mappings, redirecting their traffic through the attacker's machine.

**Key components:**

- **Raw socket creation**: Uses `AF_PACKET` with `SOCK_RAW` to bypass the kernel's network stack and send custom Ethernet frames
- **Packet structure**: Constructs packets with Ethernet header + ARP payload totaling 42 bytes
- **ARP reply spoofing**: Sets `ar_op` to `ARPOP_REPLY` and crafts responses claiming ownership of the target IP
- **Broadcast targeting**: Uses broadcast MAC (`0xff:ff:ff:ff:ff:ff`) to ensure all devices receive the poisoned ARP information
- **Continuous transmission**: Sends packets every 2 seconds to maintain the poisoned state

**Attack mechanism:**

1. Parses command-line arguments for interface, target IP, and spoofed MAC
2. Creates raw packet socket with ARP protocol type
3. Constructs malicious ARP reply claiming the target IP belongs to the specified MAC
4. Broadcasts these replies continuously to poison network ARP caches
5. Results in traffic redirection as devices route packets to the attacker's MAC

This tool requires root privileges due to raw socket usage and is designed for network security testing or malicious network interception. The continuous broadcast ensures the ARP cache entries don't expire and revert to legitimate mappings.
