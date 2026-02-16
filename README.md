Secure Remote Network Lab Project
Documentation
Author: Denarion Tyus
1. Project Overview
This project documents the design and implementation of a segmented enterprise-style network lab
using Cisco routing and switching technologies, Raspberry Pi, and Tailscale for secure remote
access. The objective was to simulate real-world infrastructure management with VLAN
segmentation, NAT, inter-VLAN routing, and secure remote connectivity without exposing internal
services to the public internet.
2. Network Architecture
• Remote Client (MacBook)
• Tailscale Encrypted Mesh VPN
• Raspberry Pi 5 (Subnet Router & Jumpbox)
• Cisco 2811 Router (NAT + Inter-VLAN Routing)
• Cisco 2960 Switch (VLAN Segmentation)
• Lab Devices across VLAN 10, 20, and 30
3. VLAN & IP Addressing Design
• VLAN 10 – 10.10.10.0/24 – Management Network
• VLAN 20 – 10.10.20.0/24 – User LAN
• VLAN 30 – 10.10.30.0/24 – Isolated Network
• 802.1Q trunk configured between router and switch
• Router-on-a-stick architecture using subinterfaces
4. NAT Configuration
NAT overload was configured on the Cisco 2811 router to allow internal VLAN networks to access
the internet via FastEthernet0/0. Access Control List 1 permits internal subnets for NAT translation
5. Secure Remote Access Design
• Tailscale installed on Raspberry Pi
• IP forwarding enabled (net.ipv4.ip_forward=1)
• Subnet route 10.10.10.0/24 advertised
• Route approved in Tailscale Admin Console
• Remote SSH and Telnet access to lab devices via hotspot
6. Security Hardening Measures
• SSH Version 2 enabled on switch
• VTY access restricted to VPN network
• No direct port forwarding exposed to internet
• Management VLAN isolated
• Centralized jumpbox architecture via Raspberry Pi
7. Technologies & Tools Used
• Cisco IOS 12.4 (2811 Router)
• Cisco IOS 15.2 (2960 Switch)
• Raspberry Pi OS (Debian Bookworm)
• Tailscale (WireGuard-based VPN)
• iptables / IP Forwarding
• SSH, Telnet, VLAN Trunking (802.1Q)
8. Lessons Learned
During implementation, CGNAT limitations prevented traditional port forwarding. This led to
adopting a mesh VPN architecture using Tailscale, which eliminated inbound firewall and NAT
complexity. The project reinforced understanding of routing, NAT, VLAN segmentation, and secure
remote infrastructure design.
9. Future Enhancements
• Automated configuration backups using Netmiko
• Centralized logging (Syslog)
• SNMP monitoring
• Firewall rule hardening
• Cloud networking integration
