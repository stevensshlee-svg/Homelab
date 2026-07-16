## Background and Evolution

My original network ran on a Netgear Nighthawk MR80 mesh system. It handled basic routing but as a consumer router it offered no real firewall control, no VLAN support, and no VPN capability. I decided to replace it with a Netgate 2100 router running pfSense.

At this point my network stack was my AT&T modem, MR80 and satellites, and the Netgate 2100 router. While the new router provided the firewall and VPN capabilities I was looking for, it did not offer any wireless support. I configured the AT&T modem into bridge mode to prevent double NAT issues and repurposed the MR80 into AP mode to avoid routing conflicts. The satellite was backhauled via Cat6 to address the wireless reliability issues I was having with the original wireless backhaul.

With the Netgate 2100 I was able to create ACLs preventing my Ring cameras and other IoT devices from communicating with other devices on the network. This addressed the immediate problem but I wasn't satisfied because everything was still on a flat network and the IoT devices weren't truly isolated in their own segment. The Netgate 2100 can route between VLANs and enforce firewall policy per VLAN interface, but it could not assign individual devices to VLANs the way a Layer 2 switch can. Without a managed switch handling Layer 2 tagging, true VLAN segmentation wasn't possible.

To address this I added the TP-Link TL-SG108PE managed switch along with a TP-Link Omada EAP610 to provide VLAN-aware wireless access for IoT devices. With a Layer 2 switch handling VLAN tagging, the Netgate 2100 could now provide inter-VLAN routing and enforce firewall policies between segments, giving me the ability to truly isolate devices rather than just restrict them with ACLs.

Coverage however was still an issue. The large staircase in the center of the house, multiple end hosts spread across both floors, and a single AP meant wireless coverage was inconsistent. This led to the current wireless design.

## Current Network Stack

**ISP**
- AT&T (modem in bridge mode)

**Firewall/Router**
- Netgate 2100 running pfSense

**Switch**
- TP-Link TL-SG108PE 8-port managed switch with PoE+

**Wireless**
- Netgear MR80 (main unit, downstairs) — AP mode, native 
  VLAN, trusted devices
- Netgear MR80 (satellite, upstairs) — wired ethernet 
  backhaul, extends trusted VLAN coverage to upper floor
- TP-Link Omada EAP610 (downstairs) — IoT VLAN, Ring 
  cameras and smart home devices
