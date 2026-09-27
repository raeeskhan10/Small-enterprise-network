# Small Enterprise Network

A Cisco Packet Tracer lab designed to simulate a small enterprise network using VLAN segmentation, VLSM subnetting, 802.1Q trunking, and inter-VLAN routing.

## Project Objectives

The goal of this project is to build and configure a functional enterprise-style network while practicing core networking concepts and troubleshooting.

The network is being built in stages and will eventually include:

- VLAN segmentation
- VLSM subnetting
- 802.1Q trunking
- Access port configuration
- Inter-VLAN routing
- DHCP
- DNS
- ACLs
- NAT
- Network troubleshooting

## Current Network Design

The network currently uses a Cisco 3560 multilayer switch as the core switch and Cisco 2960 switches at the access layer.

Departments are separated into VLANs:

| VLAN | Department | Network |
|------|------------|---------|
| 10 | Engineering | 10.10.0.0/25 |
| 20 | Sales | 10.10.0.128/26 |
| 30 | HR | 10.10.0.192/27 |
| 40 | Management | 10.10.0.224/28 |
| 50 | Servers | 10.10.0.240/28 |

## Inter-VLAN Routing

CORE-SW1 performs Layer 3 routing between the VLANs using Switch Virtual Interfaces (SVIs).

The first usable address of each subnet is used as the default gateway.

| VLAN | Default Gateway |
|------|-----------------|
| 10 | 10.10.0.1 |
| 20 | 10.10.0.129 |
| 30 | 10.10.0.193 |
| 40 | 10.10.0.225 |

Inter-VLAN connectivity has been successfully tested using ICMP ping.

## Troubleshooting

During configuration and testing, I encountered and resolved several issues, including:

- Incorrect physical interface selection when configuring trunk links
- Incorrect SVI addressing during initial configuration
- Incorrect endpoint IP addressing that prevented local and inter-VLAN communication

These issues were identified using Cisco IOS verification commands and connectivity testing.

## Documentation

Detailed project documentation is available in the `documentation` folder:

- `ip-addressing.md` — VLSM addressing plan
- `network-configuration.md` — VLAN, trunking, SVI, and routing configuration

The Cisco Packet Tracer project file is available in the `packet-tracer` folder.

## Project Status

**In Progress**

Completed:
- VLSM addressing plan
- VLAN creation
- Access port assignments
- 802.1Q trunk configuration
- SVI configuration
- Inter-VLAN routing
- Basic connectivity testing

Next:
- Server infrastructure
- DHCP
- DNS
- ACL implementation
- NAT
- Additional testing and troubleshooting
