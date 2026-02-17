Raspberry Pi Subnet Router & Secure Remote Access Configuration
Objective

The Raspberry Pi was deployed as:

Secure remote access endpoint

Subnet router for internal VLAN network (10.10.10.0/24)

Jumpbox for infrastructure management

Overlay VPN gateway using Tailscale

This design eliminated the need for public port forwarding and mitigated ISP CGNAT limitations.

🔍 Problem Identified

The ISP connection utilized Carrier-Grade NAT (CGNAT), which prevented inbound port forwarding for traditional VPN or SSH exposure.

Result:

Direct public access to internal infrastructure was not possible.

Port forwarding on the ISP router was ineffective.

🛠️ Solution Architecture

Implemented Tailscale (WireGuard-based mesh VPN) on Raspberry Pi and enabled subnet routing.

Design Flow
Remote Client (Mac)
        │
Tailscale Encrypted Mesh
        │
Raspberry Pi (Subnet Router)
        │
10.10.10.0/24 Internal Lab Network
        │
Cisco Router + Switch
⚙️ Implementation Steps
1️⃣ Install Tailscale
curl -fsSL https://tailscale.com/install.sh | sh

Authenticate:

sudo tailscale up

Login via browser when prompted.

2️⃣ Enable IP Forwarding

Edit sysctl configuration:

sudo nano /etc/sysctl.conf

Ensure the following line exists and is uncommented:

net.ipv4.ip_forward=1

Apply changes:

sudo sysctl -p

Verification:

cat /proc/sys/net/ipv4/ip_forward

Expected output:

1
3️⃣ Enable Subnet Routing

Advertise internal VLAN network:

sudo tailscale up --advertise-routes=10.10.10.0/24 --accept-routes

Approve route in Tailscale Admin Console:

https://login.tailscale.com/admin/machines

Enable advertised route for the Raspberry Pi.

4️⃣ Client Configuration

On remote client:

tailscale up --accept-routes

Or enable “Use Subnet Routes” in GUI.

🔐 Security Considerations

No public port forwarding configured

No inbound firewall exposure

SSH access restricted to VPN-connected devices

Credentials and cryptographic keys not stored in repository

Management network isolated in VLAN 10

🧠 Key Networking Concepts Demonstrated

Overlay VPN architecture

Subnet route advertisement

IP forwarding (Layer 3 routing)

CGNAT mitigation strategy

Secure jumpbox design

Remote infrastructure management best practices

📈 Result

Successfully achieved:

Remote SSH access to:

10.10.10.1 (Router)

10.10.10.2 (Switch)

10.10.10.50 (Pi)

Full management of internal lab network from external networks (e.g., mobile hotspot)

Zero public exposure of internal services

🚀 Future Enhancements

Netmiko automation for config backups

Centralized syslog server on Pi

SNMP monitoring

Firewall rule hardening

Cloud networking extension

🔎 Architectural Impact

This design mirrors enterprise remote-access patterns:

Bastion host / Jump server model

Secure overlay networking

Zero-trust style access control

No perimeter exposure

🛡️ Security Notice

All credentials, authentication tokens, and cryptographic material have been removed from this documentation for security purposes.

📌 Author

Denarion Tyus
Network Engineer | NAT & IP Routing | DNS & DHCP | MPLS | VLANs & Inter-VLAN Routing | CCNA
