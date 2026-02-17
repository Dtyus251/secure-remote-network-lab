Network Architecture Design – Secure Remote Lab
📌 Overview

This lab was designed to simulate a segmented enterprise network with secure remote infrastructure management.

The architecture focuses on:

VLAN segmentation

Inter-VLAN routing

NAT for internet access

Secure management plane

Overlay VPN remote access

Jumpbox design pattern

🏗️ Logical Architecture
Remote Client (Mac)
        │
Tailscale Encrypted Mesh VPN
        │
Raspberry Pi (Subnet Router / Jumpbox)
        │
10.10.10.0/24 Management Network
        │
Cisco 2811 Router (NAT + Inter-VLAN Routing)
        │
Cisco 2960 Switch (VLAN Segmentation)
        │
User / Isolated Networks
🌐 IP Addressing Plan
VLAN	Subnet	Purpose
10	10.10.10.0/24	Management
20	10.10.20.0/24	User LAN
30	10.10.30.0/24	Isolated Network

Gateway addresses:

10.10.10.1 (Router subinterface)

10.10.20.1

10.10.30.1

Switch Management IP:

10.10.10.2

Raspberry Pi:

10.10.10.50

Tailscale IP: 100.65.x.x

🔄 Inter-VLAN Routing

Router-on-a-Stick design:

Single physical interface to switch

802.1Q trunk configured

Subinterfaces created per VLAN

Example:

FastEthernet0/1.10
FastEthernet0/1.20
FastEthernet0/1.30

This design mirrors enterprise branch deployments.

🌍 Internet Access Design

The router performs NAT overload using:

ip nat inside source list 1 interface FastEthernet0/0 overload

ACL 1 permits:

10.10.10.0/24

10.10.20.0/24

10.10.30.0/24

Default route:

ip route 0.0.0.0 0.0.0.0 192.168.1.254

Result:
All VLAN networks can access the internet while remaining private.

🔐 Remote Access Architecture
Problem

ISP used CGNAT, preventing port forwarding and inbound VPN.

Solution

Implemented Tailscale mesh VPN on Raspberry Pi.

Pi acts as:

VPN endpoint

Subnet route advertiser

Secure jumpbox

Subnet advertised:

10.10.10.0/24

Remote clients can securely manage:

Router

Switch

Internal devices

Without exposing any services publicly.

🛡️ Security Design Principles

No direct public exposure of infrastructure

SSH enabled on management plane only

Management VLAN separated from user VLANs

No unnecessary services enabled

Overlay VPN instead of perimeter exposure

This follows modern zero-trust networking patterns.

🧠 Engineering Decisions
Why Router-on-a-Stick?

Cost-effective segmentation for branch-style environments.

Why VLAN Separation?

Limits broadcast domains and isolates management traffic.

Why Tailscale Instead of Port Forwarding?

Mitigates CGNAT limitations and improves security posture.

Why Use a Jumpbox?

Prevents direct exposure of infrastructure devices.

📈 Scalability Considerations

The architecture supports:

Adding additional VLANs

Implementing dynamic routing (OSPF/EIGRP)

Introducing firewall policies

Integrating cloud networking

Adding monitoring and automation

🎯 Outcome

Successfully built a segmented enterprise-style lab with:

Secure remote management

Internet access via NAT

Overlay VPN connectivity

Management plane isolation

Production-style infrastructure workflow

🛡️ Security Notice

All credentials, authentication tokens, and cryptographic material have been removed from repository files.

📌 Author

Denarion Tyus
Network Engineer | NAT & IP Routing | DNS & DHCP | MPLS | VLANs & Inter-VLAN Routing | CCNA
