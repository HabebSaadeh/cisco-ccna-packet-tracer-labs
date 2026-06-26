# Cisco CCNA Packet Tracer Laboratory Portfolio

This repository contains a progressive series of networking labs designed to master Cisco IOS configuration, Layer 2 switching intelligence, VLSM subnetworking, and Layer 3 static routing mechanics. 

### Lab Infrastructure Hardware Constraints
To mimic practical, high-stability physical environments and eliminate software emulation anomalies, all topologies strictly adhere to the following physical constraints:
* **Routing Engine:** Cisco 2911 Integrated Services Router (ISR)
* **Switching Fabric:** Cisco Catalyst 2960-24TT Switch
* **Media Constraints:** 100% Copper Ethernet cabling only (`Copper Straight-through` / `Copper Cross-over`). Fiber-optic uplinks and automated MDI-X dependencies are intentionally bypassed to strictly enforce physical-layer fundamentals and manual interface pinout awareness.

---

## Laboratory Catalog

### Part 1: Small Office & Local Connectivity
* **[Lab 01: Single-Switch LAN Baseline](./Part-1-Small-Office-LAN/lab-01-switch-lan-baseline.pkt)**
    * **Core Concepts:** Switch line security (hashed secrets, Type 5 masking), interface labeling, MAC Address Table (CAM) learning cycles, and aging timers.
* **[Lab 02: Advanced Switch Diagnostics & Duplex Mismatches](./Part-1-Small-Office-LAN/lab-02-switch-duplex-diagnostics.pkt)**
    * **Core Concepts:** Manual speed/duplex manipulation, autonegotiation fallback hazards, and interface error troubleshooting (Runts, CRC errors, late collisions).
* **[Lab 03: Inter-Subnet Edge Gateway Deployment](./Part-1-Small-Office-LAN/lab-03-edge-gateway-deployment.pkt)**
    * **Core Concepts:** Router interface initialization, Default Gateway configurations, and analyzing the "first packet drop" phenomenon caused by ARP resolution.
* **[Lab 04: Classless Addressing & VLSM Design](./Part-1-Small-Office-LAN/lab-04-classless-vlsm-constraints.pkt)**
    * **Core Concepts:** Classless Inter-Domain Routing (CIDR), variable length subnetting (30-host maximum limits), and interpreting Connected (C) vs. Local (L) /32 routing table routes.

### Part 2: Medium Enterprise Topologies
* **[Lab 05: Multi-Router WAN Link Baseline](./Part-2-Medium-Enterprise/lab-05-dual-router-core.pkt)**
    * **Core Concepts:** Point-to-point /30 serial-equivalent copper inter-router links and isolating standard cross-subnet reachability failures before routing convergence.
* **[Lab 06: Next-Hop IPv4 Static Routing](./Part-2-Medium-Enterprise/lab-06-next-hop-static-routing.pkt)**
    * **Core Concepts:** Next-hop IP static route mapping, recursive routing table lookups, and Administrative Distance (AD) / Metric evaluation.
* **[Lab 07: Exit-Interface Static Routing & Proxy ARP](./Part-2-Medium-Enterprise/lab-07-exit-interface-proxy-arp.pkt)**
    * **Core Concepts:** Outbound interface static configurations, "directly connected" illusions in the routing table, and the operational hazards of Proxy ARP reliance.
* **[Lab 08: Multi-Router Ring Backbone Topology](./Part-2-Medium-Enterprise/lab-08-three-router-ring-backbone.pkt)**
    * **Core Concepts:** Redundant multi-path triangular infrastructure and deploying Fully Specified Static Routes (combining both exit interface and next-hop IP).

### Part 3: Large Corporate Edge Infrastructure
* **[Lab 09: Hub-and-Spoke Enterprise Core](./Part-3-Large-Corporate-Edge/lab-09-hub-and-spoke-enterprise.pkt)**
    * **Core Concepts:** Non-contiguous subnetting (`/23` summaries), multi-hop static routing pathways, and tracking Layer 2 MAC translation updates vs. immutable Layer 3 IP headers across router hops.
* **[Lab 10: Enterprise Edge & Gateway of Last Resort](./Part-3-Large-Corporate-Edge/lab-10-enterprise-edge-default-routing.pkt)**
    * **Core Concepts:** Public IP infrastructure boundaries (`203.0.113.0/30`), configuring candidate default static routes (`0.0.0.0/0`), and edge routing traffic steering.

### Part 4: Classless Subnetting Mastery & VLSM Design
* **[Lab 11: Fixed Class C Subdivision](./Part-4-Subnetting-and-VLSM/lab-11-fixed-class-c-subdivision.pkt)**
    * **Core Concepts:** Host capacity formula ($2^H - 2$) application, classless boundary mapping, and multi-subnet local routing logic.
* **[Lab 12: High-Speed Arbitrary Subnet Mapping](./Part-4-Subnetting-and-VLSM/lab-12-block-size-trick-mapping.pkt)**
    * **Core Concepts:** Utilizing the decimal Block Size Trick ($256 - \text{Mask}$) to dynamically isolate host subnet boundaries without binary conversion.
* **[Lab 13: Zero-Waste Router Interconnects](./Part-4-Subnetting-and-VLSM/lab-13-point-to-point-zero-waste.pkt)**
    * **Core Concepts:** Traditional `/30` point-to-point link allocations alongside modern `/31` zero-overhead interface configuration alternatives.
* **[Lab 14: Class B Subnetting Matrix Optimization](./Part-4-Subnetting-and-VLSM/lab-14-class-b-matrix-optimization.pkt)**
    * **Core Concepts:** Third-octet network boundary manipulation, block size increments across Class B spaces, and cross-subnet asset verification.
* **[Lab 15: Class A Massive Scaling Scheme](./Part-4-Subnetting-and-VLSM/lab-15-class-a-massive-scaling.pkt)**
    * **Core Concepts:** Borrowing bits at scale across a default `/8` space, defining specific host ranges out of multi-million host pools, and variable prefix application.
* **[Lab 16: Chronological VLSM Network Engineering](./Part-4-Subnetting-and-VLSM/lab-16-chronological-vlsm-design.pkt)**
    * **Core Concepts:** Managing variable subnet requirements by enforcing the chronological allocation rule (largest host demands to smallest) to prevent overlapping layout boundaries.

### Part 5: Virtual LAN Isolation & Layer 2 Segmentation
* **[Lab 17: Flat Network Broadcast Domain Audit](./Part-5-VLAN-and-Layer2-Segmentation/lab-17-flat-network-broadcast-audit.pkt)**
    * **Core Concepts:** Observing default VLAN 1 alignment, tracing broadcast frame behavior in a flat topology, and evaluating CPU interruption overhead on unsegmented hosts.
* **[Lab 18: Logical VLAN Database Creation](./Part-5-VLAN-and-Layer2-Segmentation/lab-18-logical-vlan-database-creation.pkt)**
    * **Core Concepts:** Managing the local switch database via the Cisco IOS CLI, building named logical data boundaries, and auditing allocations via `show vlan brief`.
* **[Lab 19: Access Port VLAN Binding](./Part-5-VLAN-and-Layer2-Segmentation/lab-19-access-port-vlan-binding.pkt)**
    * **Core Concepts:** Hardcoding switch port operational states (`switchport mode access`) and migrating physical host-facing interfaces into distinct broadcast segments.
* **[Lab 20: Layer 2 Broadcast Isolation Verification](./Part-5-VLAN-and-Layer2-Segmentation/lab-20-layer2-broadcast-isolation-test.pkt)**
    * **Core Concepts:** Simulating BUM (Broadcast, Unknown Unicast, Multicast) traffic constraints and validating true logical broadcast containment at the access layer.
 
### Part 6: Advanced Trunking, Native Overheads & Router-on-a-Stick (RoAS)
* **[Lab 11: Explicit Dot1q Trunking & VLAN Matrix Filters](./Part-6-Trunking-and-InterVLAN-Routing/lab-11-explicit-vlan-trunk-filtering.pkt)**
    * **Core Concepts:** Hardcoding static operational trunk modes, disabling dynamic trunking autonegotiation protocols, and modifying allowed VLAN matrices to selectively drop transit traffic.
* **[Lab 12: Native VLAN Manipulation & Mismatch Analysis](./Part-6-Trunking-and-InterVLAN-Routing/lab-12-native-vlan-mismatch-analysis.pkt)**
    * **Core Concepts:** Migrating untagged frame pathways to custom native VLAN identifiers, analyzing native mismatch console logs, and evaluating 802.1Q header structures.
* **[Lab 13: Router-on-a-Stick (RoAS) Logical Gateway Architecture](./Part-6-Trunking-and-InterVLAN-Routing/lab-13-router-on-a-stick-gateways.pkt)**
    * **Core Concepts:** Initializing single physical-to-logical interface partitions, mapping subinterfaces to corresponding VLAN IDs, and configuring discrete classless multi-subnet default gateways.
* **[Lab 14: Advanced Native Gateways on Logical Subinterfaces](./Part-6-Trunking-and-InterVLAN-Routing/lab-14-advanced-subinterface-native.pkt)**
    * **Core Concepts:** Enforcing native untagged frame termination patterns on routing engines, utilizing encapsulation native flags, and comparing physical port gateway assignments.

---

## Verification & Diagnostic Toolkit
To validate the control plane and data plane stability across these labs, the following Cisco IOS diagnostic commands are continually utilized:
```bash
show ip interface brief      # Evaluate L1 physical status and L2 protocol framing
show ip route                # Analyze routing logic, prefix lengths, and path selection
show mac address-table       # Audit Layer 2 hardware address maps on switching fabrics
show interfaces <interface>  # Investigate hardware metrics, CRC errors, and duplex performance
