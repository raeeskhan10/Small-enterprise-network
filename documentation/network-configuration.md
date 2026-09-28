# Network Configuration

## Overview

This document contains the configuration of the switches, VLANs, trunk links, and inter-VLAN routing used in the Small Enterprise Network project.

## VLAN Configuration

| VLAN | Name | Purpose |
|------|------|---------|
| 10 | ENGINEERING | Engineering department |
| 20 | SALES | Sales department |
| 30 | HR | Human Resources department |
| 40 | MANAGEMENT | Network management |
| 50 | SERVERS | Server infrastructure |

## Network Devices

| Device | Role |
|--------|------|
| CORE-SW1 | Multilayer core switch / Layer 3 routing |
| SW-ENG | Engineering access switch |
| SW-SALES | Sales access switch |
| SW-ADMIN | HR and Management access switch |

## Layer 2 Configuration

The access switches connect endpoint devices to their assigned VLANs. Trunk links connect the access switches to CORE-SW1 and carry the required VLAN traffic between switches.

### Access Port Assignments

- SW-ENG: Fa0/1–2 → VLAN 10
- SW-SALES: Fa0/1–2 → VLAN 20
- SW-ADMIN: Fa0/1–2 → VLAN 30
- SW-ADMIN: Fa0/3–4 → VLAN 40

## Inter-VLAN Routing

CORE-SW1 is a multilayer switch and performs Layer 3 routing between the VLANs using Switch Virtual Interfaces (SVIs).

| VLAN | Default Gateway |
|------|-----------------|
| VLAN 10 | 10.10.0.1 |
| VLAN 20 | 10.10.0.129 |
| VLAN 30 | 10.10.0.193 |
| VLAN 40 | 10.10.0.225 |

IP routing was enabled on CORE-SW1 to allow communication between the VLAN networks.

## Testing

Connectivity was verified using ICMP ping tests.

Testing confirmed:

- Communication between hosts within the same VLAN
- Connectivity between different VLANs
- Connectivity to each VLAN's default gateway
- Successful inter-VLAN routing through CORE-SW1

## Troubleshooting

During testing, MGMT-PC1 was initially configured with an incorrect IP address. This prevented both local VLAN communication and communication with other VLANs.

The IP configuration was corrected and connectivity was successfully restored.

This demonstrated the importance of verifying endpoint IP addressing, subnet masks, and default gateways during network troubleshooting.

## Server Infrastructure

A dedicated server VLAN was configured to provide centralized network services.

| Device | IP Address | VLAN | Purpose |
|---|---|---|---|
| SRV-DHCP-DNS | 10.10.0.242/28 | 50 | DHCP and DNS services |

The VLAN 50 SVI on CORE-SW1 uses `10.10.0.241` as the default gateway.

## DHCP Configuration

SRV-DHCP-DNS provides dynamic IPv4 addressing to client devices in VLANs 10, 20, 30, and 40.

Because the DHCP server is located in VLAN 50, DHCP relay was configured on the client VLAN SVIs using:

`ip helper-address 10.10.0.242`

This allows DHCP broadcast requests from each client VLAN to reach the centralized DHCP server.

DHCP functionality was verified by configuring client PCs for automatic addressing and confirming that each client received an address, subnet mask, default gateway, and DNS server appropriate for its VLAN.

## DNS Configuration

SRV-DHCP-DNS also provides DNS services.

An A record was created:

`server.company.local → 10.10.0.242`

DNS functionality was verified by successfully resolving and pinging `server.company.local` from client devices.

## Access Control List

An extended ACL named `SALES-RESTRICTIONS` was configured to prevent the Sales network from communicating with the HR network while permitting other IP traffic.

```text
ip access-list extended SALES-RESTRICTIONS
 deny ip 10.10.0.128 0.0.0.63 10.10.0.192 0.0.0.31
 permit ip any any

## Edge Routing

An edge router named `EDGE-R1` was added between the enterprise network and a simulated ISP.

A /30 point-to-point network connects CORE-SW1 and EDGE-R1:

| Device | Interface | IP Address |
|---|---|---|
| CORE-SW1 | Fa0/23 | 10.10.1.1/30 |
| EDGE-R1 | Gi0/0 | 10.10.1.2/30 |

CORE-SW1 uses EDGE-R1 as its default route.

EDGE-R1 contains a static route back to the internal `10.10.0.0/24` enterprise networks.

## NAT/PAT

EDGE-R1 provides NAT/PAT for internal clients accessing the simulated external network.

The outside connection uses:

| Device | Interface | IP Address |
|---|---|---|
| EDGE-R1 | Gi0/1 | 203.0.113.1/30 |
| ISP-R1 | Gi0/0 | 203.0.113.2/30 |

PAT overload allows multiple private `10.10.0.0/24` hosts to share the EDGE-R1 outside address `203.0.113.1`.

NAT translations were verified using:

`show ip nat translations`

Testing confirmed that internal hosts could successfully reach the simulated ISP network while their private addresses were translated by EDGE-R1.

## Troubleshooting

Several issues were identified and resolved during the project:

- Trunk configuration was initially applied to interfaces different from the physical uplinks.
- An incorrect SVI address was identified and corrected.
- An incorrect Management PC address prevented local and inter-VLAN connectivity.
- DHCP broadcasts required `ip helper-address` because the DHCP server resides in a different VLAN.
- The Sales-to-HR ACL did not filter as expected when applied inbound to the VLAN 20 SVI in Packet Tracer. The ACL was moved outbound to the VLAN 30 SVI, where the intended policy was successfully enforced.

## Final Verification

Final testing confirmed:

- DHCP address assignment across all client VLANs
- Inter-VLAN routing
- DNS name resolution
- Connectivity to the DHCP/DNS server
- Sales-to-HR traffic blocked by ACL
- Permitted inter-VLAN traffic remained functional
- Connectivity from internal clients to the simulated ISP
- Successful NAT/PAT translations

The completed network passed all planned connectivity and security tests.
