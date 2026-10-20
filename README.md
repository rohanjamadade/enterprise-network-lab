Enterprise Network Lab — Cisco Packet Tracer

Project Overview

This project demonstrates the design, configuration, and validation of an enterprise network using Cisco Packet Tracer. It focuses on VLAN segmentation, inter-VLAN routing, Layer 2 switching, Layer 3 routing, and edge-router configuration.

The lab simulates a small enterprise environment with separate user, finance, IT, server, voice, and management VLANs.

Project status: Internal connectivity validated; external Internet connectivity not verified.

Objectives

- Implement VLAN-based network segmentation.
- Configure access ports and 802.1Q trunk links.
- Enable inter-VLAN routing using Layer 3 switch SVIs.
- Configure static routing between the core switch and edge router.
- Configure NAT/PAT on the edge router.
- Validate connectivity between PCs, VLAN gateways, the server, and the edge router.
- Document limitations and possible future enhancements.

Network Topology

The lab uses the following topology:

              Cloud-PT
                  |
               EDGE-R1
                  |
               CORE-SW1
              /    |    \
       ACCESS-SW1  SRV1  ACCESS-SW2
          /  \             /  \
        PC0  PC1          PC2  PC3

The diagram is a logical representation of the lab. Refer to the topology screenshot in this repository for the actual Packet Tracer connections.

Devices used

Device| Role
Cloud-PT| Simulated external network connection
EDGE-R1 — Cisco 2911| Edge routing and NAT/PAT configuration
CORE-SW1 — Catalyst 3650| Layer 3 switching and inter-VLAN routing
ACCESS-SW1 — Catalyst 2960| End-device access and VLAN connectivity
ACCESS-SW2 — Catalyst 2960| End-device access and VLAN connectivity
PC0–PC3| Client endpoints
SRV1| Internal server

VLAN Design and IP Addressing

The internal network uses the "192.168.0.0/16" address space, with separate "/24" subnets for each VLAN.

VLAN ID| VLAN Name| Subnet| Default Gateway
10| USERS| 192.168.10.0/24| 192.168.10.1
20| FINANCE| 192.168.20.0/24| 192.168.20.1
30| IT| 192.168.30.0/24| 192.168.30.1
40| SERVERS| 192.168.40.0/24| 192.168.40.1
50| VOICE| 192.168.50.0/24| 192.168.50.1
99| MANAGEMENT| 192.168.99.0/24| 192.168.99.1

Endpoint addressing

Device| IP Address| VLAN
PC0| 192.168.10.10/24| 10
PC1| 192.168.30.10/24| 30
PC2| 192.168.20.10/24| 20
PC3| 192.168.10.11/24| 10
SRV1| 192.168.40.10/24| 40

Each endpoint uses the corresponding VLAN SVI address as its default gateway.

Core-to-Edge Connectivity

A dedicated point-to-point transit network connects the Layer 3 core switch to the edge router.

Device| Interface| IP Address
CORE-SW1| Gi1/0/1| 10.255.255.1/30
EDGE-R1| Gi0/1| 10.255.255.2/30

The core switch uses a default route toward EDGE-R1. The edge router has a static return route for the internal enterprise network.

Routing configuration

On CORE-SW1:

ip routing
ip route 0.0.0.0 0.0.0.0 10.255.255.2

On EDGE-R1:

ip route 192.168.0.0 255.255.0.0 10.255.255.1

The transit link and connectivity between CORE-SW1 and EDGE-R1 were validated.

Switching Configuration

VLANs and access ports

VLANs are configured to separate users, finance, IT, servers, voice, and management traffic.

Access ports assign endpoint devices to their designated VLANs. The access switches connect to CORE-SW1 using trunk links.

Trunk configuration

The switch uplinks use IEEE 802.1Q trunking with native VLAN 99. The configured allowed VLANs are:

- VLAN 10 — USERS
- VLAN 20 — FINANCE
- VLAN 30 — IT
- VLAN 40 — SERVERS
- VLAN 50 — VOICE
- VLAN 99 — MANAGEMENT

Trunk operation and VLAN transport were verified during lab testing.

Inter-VLAN Routing

CORE-SW1 performs Layer 3 routing between VLANs using Switched Virtual Interfaces (SVIs).

The configured gateway addresses are:

VLAN 10  → 192.168.10.1
VLAN 20  → 192.168.20.1
VLAN 30  → 192.168.30.1
VLAN 40  → 192.168.40.1
VLAN 50  → 192.168.50.1
VLAN 99  → 192.168.99.1

The SVIs were confirmed to be up/up during validation. Successful communication between client VLANs and the internal server demonstrated that inter-VLAN routing was functioning.

NAT/PAT Configuration

NAT overload (PAT) is configured on EDGE-R1 to translate internal enterprise traffic using the external interface address.

The following configuration was applied:

access-list 1 permit 192.168.0.0 0.0.255.255

interface GigabitEthernet0/1
 ip nat inside

interface GigabitEthernet0/0
 ip nat outside

ip nat inside source list 1 interface GigabitEthernet0/0 overload

Validation status: Configuration is present, but successful NAT translation has not been demonstrated. See the limitations section below.

Validation and Test Results

The following tests were performed in Cisco Packet Tracer.

Validation test| Result
VLANs and access-port assignments| Verified
Trunk links and allowed VLANs| Verified
VLAN SVI status| Verified up/up
PC-to-default-gateway connectivity| Successful
PC-to-internal-server connectivity| Successful
Inter-VLAN routing| Successful
CORE-SW1-to-EDGE-R1 transit connectivity| Successful
Static routing configuration| Verified
NAT/PAT configuration| Present; translation not verified
External Internet connectivity| Not verified

Key verification commands

The following commands are useful for reviewing the configuration and troubleshooting the lab.

show vlan brief
show interfaces trunk
show ip interface brief
show ip route
show running-config
show ip nat translations
show ip nat statistics

On the client PCs, the following commands can be used to validate addressing and connectivity:

ipconfig
ping 192.168.10.1
ping 192.168.40.10
ping 10.255.255.2

Use the appropriate gateway address for each client's VLAN.

External Connectivity and NAT/PAT Limitations

Why external connectivity is not verified

The lab's Cloud-PT device is connected to EDGE-R1, but connecting to a simulated cloud does not, by itself, provide a working ISP connection.

The following prerequisites are missing or unconfirmed:

1. External IP configuration

EDGE-R1's GigabitEthernet0/0 interface was observed in an up/up state but had no IP address assigned. An operational external connection requires valid upstream interface addressing.

2. Upstream gateway

No functioning ISP gateway or upstream route has been configured and verified. The Cloud-PT DSL-to-Ethernet mapping does not establish a working Internet service by itself.

3. NAT/PAT validation

NAT overload is configured, but the NAT translation table remained empty and the translation hit counter was zero during testing. Without a suitable upstream path and generated traffic, successful end-to-end translation cannot be demonstrated.

Scope decision

The existing topology is intentionally retained without adding an ISP router or additional links. The project therefore demonstrates internal enterprise connectivity and edge-routing configuration, but it does not claim functional Internet access or successful end-to-end NAT/PAT operation.

External connectivity can be implemented and validated as a future enhancement.

Key Learning Outcomes

This project provides practical experience with:

- Enterprise VLAN segmentation and access-port assignment.
- IEEE 802.1Q trunking and native VLAN configuration.
- Layer 3 switching and inter-VLAN routing.
- Static routing between a core switch and edge router.
- NAT/PAT configuration concepts.
- Connectivity testing and routing troubleshooting.
- Documenting verified results, configuration status, and technical limitations.

Project Files

- Network topology screenshot: Visual reference for the lab layout.
- Cisco Packet Tracer project file (".pkt"): Network topology and device configurations, if included in the repository.
- README.md: Architecture, addressing plan, configuration overview, validation results, and limitations.

Future Enhancements

Potential extensions to this lab include:

- Add an ISP router or a suitable upstream network simulation.
- Configure valid external addressing and a reachable upstream gateway.
- Generate outbound traffic and validate NAT/PAT translations.
- Verify end-to-end connectivity to a simulated external destination.
- Add ACLs and test traffic restrictions between VLANs.
- Expand the lab with monitoring, configuration backup, and network automation.

Author

Rohan Jamadade

Network Engineering | Enterprise Infrastructure | Network Automation

GitHub: "rohanjamadade" (https://github.com/rohanjamadade)