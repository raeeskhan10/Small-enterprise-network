# IP Addressing Plan

## Network Overview

Apex Technologies has been allocated the private IPv4 network:

**10.10.0.0/16**

Variable Length Subnet Masking (VLSM) was used to divide the address space
based on the host requirements of each department while minimizing wasted
IP addresses.

## VLAN and Subnet Allocation

| VLAN | Department | Hosts Required | Network | CIDR | Subnet Mask | First Usable | Last Usable | Broadcast |
|------|------------|---------------|---------|------|-------------|--------------|-------------|-----------|
| 10 | Engineering | 100 | 10.10.0.0 | /25 | 255.255.255.128 | 10.10.0.1 | 10.10.0.126 | 10.10.0.127 |
| 20 | Sales | 50 | 10.10.0.128 | /26 | 255.255.255.192 | 10.10.0.129 | 10.10.0.190 | 10.10.0.191 |
| 30 | HR | 25 | 10.10.0.192 | /27 | 255.255.255.224 | 10.10.0.193 | 10.10.0.222 | 10.10.0.223 |
| 40 | Management | 10 | 10.10.0.224 | /28 | 255.255.255.240 | 10.10.0.225 | 10.10.0.238 | 10.10.0.239 |
| 50 | Servers | 10 | 10.10.0.240 | /28 | 255.255.255.240 | 10.10.0.241 | 10.10.0.254 | 10.10.0.255 |

## Design Decisions

VLSM was used instead of assigning the same subnet size to every department.
Each VLAN received the smallest subnet capable of supporting its required
number of hosts.

The subnets were allocated from largest to smallest to efficiently use the
available address space and simplify subnet planning.
