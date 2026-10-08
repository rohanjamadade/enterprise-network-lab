# Enterprise Network Lab: Cisco Packet Tracer

A campus-style enterprise network built and documented in Cisco Packet Tracer. It covers VLAN segmentation, inter-VLAN routing, redundancy, DHCP, routing and troubleshooting.

> **Note:** This is a lab environment with fictional, sanitized addressing. It is not production infrastructure.

## Topology

```
                Internet
                    |
                 Router
                    |
           Core L3 Switch (Catalyst 3650)
            /       |        \
     Access SW1  Access SW2   Server
        |            |
   Users / PCs   Users / PCs
```

## VLAN and IP Design

| VLAN | Name | Subnet | Gateway |
|------|------|--------|---------|
| 10 | USERS | 192.168.10.0/24 | 192.168.10.1 |
| 20 | FINANCE | 192.168.20.0/24 | 192.168.20.1 |
| 30 | IT | 192.168.30.0/24 | 192.168.30.1 |
| 40 | SERVERS | 192.168.40.0/24 | 192.168.40.1 |
| 50 | VOICE | 192.168.50.0/24 | 192.168.50.1 |
| 99 | MANAGEMENT | 192.168.99.0/24 | 192.168.99.1 |

## Scope

**Layer 2:** VLANs, access and trunk ports, 802.1Q, native VLAN, allowed VLANs, MAC table, STP/RSTP, root bridge, PortFast, BPDU Guard, EtherChannel

**Layer 3:** SVIs, inter-VLAN routing, static routing, OSPF, default route, routing table verification

**Network services:** DHCP, DHCP relay, DNS concepts, IP addressing and subnetting

**Security:** Port security, ACLs, management VLAN, SSH, segmentation

## Troubleshooting Scenarios

- VLAN mismatch
- Trunk misconfiguration
- Native VLAN mismatch
- VLAN not allowed on trunk
- Incorrect default gateway
- Missing route
- DHCP failure
- STP issue
- Duplicate IP
- End-to-end connectivity failure

## Repository Structure

```
enterprise-network-lab/
├── README.md
├── topology/
├── configs/
├── verification/
└── troubleshooting/
```

## Project Status

- [x] Topology and VLAN/IP design
- [ ] Switch configurations
- [ ] Verification outputs
- [ ] Troubleshooting scenarios

## Tools

Cisco Packet Tracer, Cisco IOS CLI