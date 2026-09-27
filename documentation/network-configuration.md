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
