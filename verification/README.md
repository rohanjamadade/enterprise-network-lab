Network Verification Guide

Purpose

This document describes the commands and tests used to verify the Enterprise Network Lab built in Cisco Packet Tracer.

The goal is to validate VLAN configuration, trunk links, spanning tree, routing, DHCP, connectivity, and basic network security.

Note: Results must be collected from the actual lab. No test is marked as passed until it has been performed.

1. VLAN Verification

Devices: CORE-SW1, ACCESS-SW1, ACCESS-SW2

show vlan brief

Verify:

- VLANs 10, 20, 30, 40, 50, and 99 are created.
- VLAN names match the addressing plan.
- Access ports are assigned to the intended VLANs.

2. Trunk Verification

Devices: CORE-SW1, ACCESS-SW1, ACCESS-SW2

show interfaces trunk

Verify:

- Expected trunk links are operational.
- Native VLAN is 99 on both ends.
- Required VLANs are allowed across each trunk.

3. Spanning Tree Verification

Devices: All switches

show spanning-tree
show spanning-tree vlan 10

Verify:

- Spanning tree is operating.
- The root bridge is the intended switch.
- Port roles and states are appropriate for the topology.

4. Interface Verification

Devices: All switches and EDGE-R1

show ip interface brief
show interfaces status

Verify:

- Required interfaces are up.
- Interface IP addresses are correct.
- Connected ports match the topology diagram.

5. Routing Verification

Devices: CORE-SW1 and EDGE-R1

show ip route
show ip protocols
show ip ospf neighbor

Verify:

- Connected VLAN networks appear in the routing table when their SVIs are operational.
- The core and edge router have the expected routes.
- OSPF neighbors form where configured and directly connected.

6. DHCP Verification

Device: CORE-SW1, if DHCP is configured there

show ip dhcp binding
show ip dhcp pool

Verify:

- Expected DHCP pools exist.
- Clients receive addresses from the correct subnet.
- Assigned addresses do not conflict with reserved addresses.

7. MAC Address and ARP Verification

Devices: Switches and Layer 3 devices

show mac address-table
show ip arp

Verify:

- Connected devices' MAC addresses are learned.
- ARP entries appear for reachable IPv4 neighbors where applicable.

8. Port Security Verification

Devices: ACCESS-SW1 and ACCESS-SW2

show port-security
show port-security interface fastethernet0/1

Verify:

- Port security is enabled on intended access ports.
- Maximum MAC address limits and violation actions are correct.
- Violation counters are reviewed when testing security behavior.

9. Connectivity Tests

From a PC command prompt:

ipconfig
ping 192.168.10.1
ping 192.168.40.10
tracert 192.168.40.10

Use the gateway matching the PC's assigned VLAN. The addresses above are examples for the planned lab.

Verify:

- The PC has a valid IP address and default gateway.
- Its local VLAN gateway responds.
- Server reachability and routing work when the relevant configuration is complete.

10. Recording Results

For every test, record:

Field| Description
Device| Device tested
Command/Test| Command or connectivity test performed
Expected Result| Intended behavior
Actual Result| Observed output
Status| Pass, Fail, or Not Tested
Corrective Action| Fix applied, if required

Important Notes

- This guide is a verification checklist, not proof of successful operation.
- Record real command output from Cisco Packet Tracer.
- Commands can vary by device model and Packet Tracer version.
- All IP addresses and device names are fictional and intended for a lab environment.
