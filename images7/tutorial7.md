### Wireshark ARP Capture Overview

![Wireshark ARP List](arp-list.png)

#### Packet Analysis:
* **Captured Traffic:** The capture shows background IPv6 ICMPv6 messages, DHCP configuration handshakes, and periodic ARP broadcast requests across the local subnet.
* **Key Observations:**
  * **Packet 11:** Broadcast ARP query from `PCSSystemtec_5e:01:7c` (`192.168.1.1`) inquiring *"Who has 192.168.1.105? Tell 192.168.1.1"*.
  * **Packet 22:** Broadcast ARP query from `PCSSystemtec_2d:f8:5a` (`192.168.1.105`) inquiring *"Who has 192.168.1.1? Tell 192.168.1.105"*.
  * **Packet 35:** Unicast ARP reply from `08:00:27:5e:01:7c` confirming that `192.168.1.1` is located at hardware MAC address `08:00:27:5e:01:7c`.


### Packet 22: ARP Request Deep Dive & Encapsulation

![Packet 22 Details](arp-packet-22.png)

#### Addressing & Protocol Breakdown:
* **Frame Layer (Physical/Data Link):**
  * **Frame Length:** 60 bytes on wire (480 bits)
  * **Capture Length:** 60 bytes
  * **Encapsulation Type:** Ethernet (1)
* **Ethernet II Layer:**
  * **Destination MAC:** `ff:ff:ff:ff:ff:ff` (Broadcast)
  * **Source MAC:** `08:00:27:2d:f8:5a` (`PCSSystemtec_2d:f8:5a`)
  * **EtherType:** `0x0806` (Address Resolution Protocol)
  * **Ethernet Padding:** 18 bytes of null padding added to meet the minimum Ethernet frame specification (60 bytes excluding 4-byte FCS).
* **Address Resolution Protocol (ARP) Request Layer:**
  * **Hardware Type:** Ethernet (`1`)
  * **Protocol Type:** IPv4 (`0x0800`)
  * **Hardware Size:** 6 bytes
  * **Protocol Size:** 4 bytes
  * **Opcode:** Request (`1`)
  * **Sender MAC Address:** `08:00:27:2d:f8:5a`
  * **Sender IP Address:** `192.168.1.105`
  * **Target MAC Address:** `00:00:00:00:00:00` (All zeros, as it is unknown and being requested)
  * **Target IP Address:** `192.168.1.1`

#### Explanation:
Before the host `192.168.1.105` can transmit an IP packet to the gateway `192.168.1.1`, it must determine the gateway's Layer 2 MAC address. Since it is not currently stored in its local ARP table, the host sends this Layer 2 broadcast query. All machines receive the frame, but only the host assigned `192.168.1.1` processes and responds to it.
