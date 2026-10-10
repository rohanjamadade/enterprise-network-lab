# VLAN and IP Addressing Plan

## Overview

This document defines the VLAN segmentation and IPv4 addressing plan for the Enterprise Network Lab.

All IP addresses and device names are fictional and intended for a lab environment.

## VLAN Table

| VLAN ID | VLAN Name | Subnet | Default Gateway | Purpose |
|---:|---|---|---|---|
| 10 | USERS | 192.168.10.0/24 | 192.168.10.1 | General user devices |
| 20 | FINANCE | 192.168.20.0/24 | 192.168.20.1 | Finance department devices |
| 30 | IT | 192.168.30.0/24 | 192.168.30.1 | IT department devices |
| 40 | SERVERS | 192.168.40.0/24 | 192.168.40.1 | Server infrastructure |
| 50 | VOICE | 192.168.50.0/24 | 192.168.50.1 | Voice services |
| 99 | MANAGEMENT | 192.168.99.0/24 | 192.168.99.1 | Network device management |

## Subnet Details

- Addressing: IPv4
- Subnet mask: 255.255.255.0
- Prefix length: /24
- Total addresses per subnet: 256
- Usable host addresses per subnet: 254

## Network Design

- VLANs provide logical separation between device groups.
- Access ports connect end-user devices to their assigned VLANs.
- Trunk links carry multiple VLANs between switches.
- The Layer 3 core switch provides inter-VLAN routing through switched virtual interfaces (SVIs).
- The management VLAN is reserved for network device management.

## Planned Device Management Addresses

| Device | Management IP |
|---|---|
| CORE-SW1 | 192.168.99.1 |
| ACCESS-SW1 | 192.168.99.11 |
| ACCESS-SW2 | 192.168.99.12 |

## Implementation Notes

This document describes the planned addressing design. Actual VLAN operation, SVI status, DHCP assignment, and end-to-end connectivity must be verified in Cisco Packet Tracer before they are considered operational.

The VOICE VLAN is included in the design but does not yet imply that voice devices or voice services have been configured.
