# Homelab
A self-hosted home network and lab environment built for hands on practice in networking, infrastructure management, Active Directory, and security. 

## Main Environment Overview
- Proxmox VE hypervisor on a refurbished HP EliteDesk 800 G5 Mini
- pfSense firewall with VLAN segmentation (VLAN 10 servers, VLAN 20 trusted hosts on WiFi, and VLAN 30 Iot)
- TP-Link 8 port managed switch with POE+ (TL-SG108PE)
- Self-hosted services: Pi-hole, Vaultwarden, Nginx Proxy Manager, Zabbix, EVE-NG, Home Assistant

## Secondary Environment
- Virtualbox running Windows Server 2019 and Windows 10
- Active Directory lab simulating a two-site enterprise domain
- AD administration, GPO deployment, network file-share, and PXE boot

## Repository Structure
- infrastructure/ - hardware, network design, and platform decisions
- services/ - self-hosted application deployments and configs
- sandboxes/ - Active Directory, networking, and Linux sandboxes

Insert image
