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

---

## Verification & Diagnostic Toolkit
To validate the control plane and data plane stability across these labs, the following Cisco IOS diagnostic commands are continually utilized:
```bash
show ip interface brief      # Evaluate L1 physical status and L2 protocol framing
show ip route                # Analyze routing logic, prefix lengths, and path selection
show mac address-table       # Audit Layer 2 hardware address maps on switching fabrics
show interfaces <interface>  # Investigate hardware metrics, CRC errors, and duplex performance
