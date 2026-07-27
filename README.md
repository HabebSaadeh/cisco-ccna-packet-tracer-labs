# My Cisco CCNA Infrastructure Portfolio

Welcome! This repository documents my practical learning journey as I learn networking and prepare to break into the field. Instead of just reading theory, I built these 100 (They are less than that at the moment, I said 100 just to challenge myself) topologies to understand the real-world networking, configurations, and cause-and-effect of engineering choices. 

---

## Laboratory Catalog

### Part 1: Small Office & Local Connectivity

* **[Lab 01: Single-Switch LAN Baseline](./Part-1-Small-Office-LAN/lab-01-switch-lan-baseline.pkt)**
    * **What I did:** Built a secure local network layer from scratch. Initialized line access, locked down Privileged EXEC mode with Type 5 MD5 hashing, and masked plain-text credentials.
    * **The Validation:** Verified how the switch dynamically maps physical source hardware addresses to internal ports and monitored table properties.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW1# show mac address-table dynamic
                  Mac Address Table
        -------------------------------------------
        Vlan    Mac Address       Type        Ports
        ----    -----------       --------    -----
          1    0004.9ae9.1322    DYNAMIC     Fa0/2
          1    0030.f250.c189    DYNAMIC     Fa0/4
          1    0060.5cd5.d8ed    DYNAMIC     Fa0/1
          1    00d0.bc28.d912    DYNAMIC     Fa0/3
        ```
        </details>

* **[Lab 02: Advanced Switch Port Diagnostics & Duplex Mismatches](./Part-1-Small-Office-LAN/lab-02-switch-duplex-diagnostics.pkt)**
    * **What I did:** Intentionally forced a speed and duplex configuration anomaly between two interconnecting switches (one side hardcoded to full-duplex, the neighbor left to fall back to half-duplex).
    * **The Validation:** Analyzed why the link deceptively displays an "up/up" state while tracking physical port interface counters to watch packet drops, late collisions, and CRC errors accumulate.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW1# show interfaces fastEthernet 0/24
        FastEthernet0/1 is up, line protocol is up (connected)
          Full-duplex, 100Mb/s, media type is 100BaseTX
          1423 input errors, 1423 CRC, 0 frame
          0 output errors, 0 collisions, 0 late collisions

        SW2# show interfaces fastEthernet 0/24
        FastEthernet0/1 is up, line protocol is up (connected)
          Half-duplex, 100Mb/s, media type is 100BaseTX
          0 input errors, 0 CRC, 0 frame
          915 output errors, 1240 collisions, 915 late collisions
        ```
        </details>

* **[Lab 03: Inter-Subnet Edge Gateway Deployment](./Part-1-Small-Office-LAN/lab-03-edge-gateway-deployment.pkt)**
    * **What I did:** Brought up a localized corporate file server on a completely separate subnet and bound the default gateway interfaces on a 2911 router.
    * **The Validation:** Tracked the exact execution mechanics of end-host ARP requests and documented why the first ICMP ping packet invariably drops (`.!!!!`) while waiting for address resolution.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        PC-A> ping 192.168.2.10
        Pinging 192.168.2.10 with 32 bytes of data:
        Request timed out.
        Reply from 192.168.2.10: bytes=32 time=10ms TTL=127
        
        R1# show arp
        Protocol  Address          Age (min)  Hardware Addr   Type   Interface
        Internet  192.168.1.50             2  0002.14a3.11b2  ARPA   GigabitEthernet0/0
        Internet  192.168.2.10             0  000d.bd61.c11a  ARPA   GigabitEthernet0/1
        ```
        </details>

* **[Lab 04: Classless Addressing & VLSM Design](./Part-1-Small-Office-LAN/lab-04-classless-vlsm-constraints.pkt)**
    * **What I did:** Took a flat network block and carved it out using custom classless prefix boundaries to support a tight 30-host maximum per subnet segment.
    * **The Validation:** Audited the Layer 3 routing database to differentiate how the router tracks parent classless summary routes versus its internal `/32` host paths.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R1# show ip route connected
             192.168.50.0/24 is variably subnetted, 4 subnets, 2 masks
        C       192.168.50.0/27 is directly connected, GigabitEthernet0/0
        L       192.168.50.1/32 is directly connected, GigabitEthernet0/0
        ```
        </details>

---

### Part 2: Medium Enterprise Topologies

* **[Lab 05: Multi-Router WAN Link Baseline](./Part-2-Medium-Enterprise/lab-05-dual-router-core.pkt)**
    * **What I did:** Linked two regional corporate branch offices using a tight, isolated `/30` point-to-point transit subnet between the router interfaces.
    * **The Validation:** Proved why cross-subnet communications fail immediately out of the box despite "up/up" interface configurations, due to the empty state of remote networks in the routing table.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R1# show ip interface brief | include Gigabit
        GigabitEthernet0/0     192.168.12.1    YES manual up                    up
        GigabitEthernet0/1     10.1.1.1        YES manual up                    up
        
        R1# show ip route
        -- Output truncated: Only directly connected subnets visible --
        ```
        </details>

* **[Lab 06: Next-Hop IPv4 Static Routing](./Part-2-Medium-Enterprise/lab-06-next-hop-static-routing.pkt)**
    * **What I did:** Manual traffic engineering between branch sites using next-hop IP static route statements.
    * **The Validation:** Examined the routing table logic to show how next-hop routing forces recursive lookups, and verified the default Administrative Distance (AD) metrics of static routes.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R1# show ip route static
        Codes: L - local, C - connected, S - static
        
        S    10.2.2.0/24 [1/0] via 192.168.12.2
        ```
        </details>

* **[Lab 07: Exit-Interface Static Routing & Proxy ARP](./Part-2-Medium-Enterprise/lab-07-exit-interface-proxy-arp.pkt)**
    * **What I did:** Cleared out the previous next-hop routes and re-engineered path maps using only the outbound physical exit interface designator.
    * **The Validation:** Witnessed the deceptive "directly connected" illusion the router displays in the routing table and analyzed how this forces the device to rely completely on Proxy ARP resolution for every destination.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R1# show ip route static
        Codes: L - local, C - connected, S - static
        
        S    10.2.2.0/24 is directly connected, GigabitEthernet0/0
        ```
        </details>

* **[Lab 08: Multi-Router Ring Backbone Topology](./Part-2-Medium-Enterprise/lab-08-three-router-ring-backbone.pkt)**
    * **What I did:** Built a highly available, triangular three-router core ring infrastructure linking Alpha, Beta, and Gamma sites.
    * **The Validation:** Programmed and verified Fully Specified Static Routes (mapping both the local exit port and the exact neighbor gateway IP) to ensure predictable, deterministic loop-free forwarding.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R_Alpha# show ip route static
        S    192.168.20.0/24 [1/0] via 10.12.0.2, GigabitEthernet0/0
        S    192.168.30.0/24 [1/0] via 10.31.0.2, GigabitEthernet0/1
        ```
        </details>

---

### Part 3: Large Corporate Edge Infrastructure

* **[Lab 09: Hub-and-Spoke Enterprise Core](./Part-3-Large-Corporate-Edge/lab-09-hub-and-spoke-enterprise.pkt)**
    * **What I did:** Provisioned a central corporate headquarters (Hub) routing core acting as the single transit path for two isolated branch operations (Spokes) using a wide `/23` address block.
    * **The Validation:** Tracked data packets mid-transit to verify the absolute immutability of Layer 3 IP headers versus the constant rewrite and translation of Layer 2 MAC address frames at every single router hop.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R_HQ# show ip route static
        S    172.16.2.0/24 [1/0] via 10.0.1.2
        S    172.16.3.0/24 [1/0] via 10.0.2.2
        ```
        </details>

* **[Lab 10: Enterprise Edge & Gateway of Last Resort](./Part-3-Large-Corporate-Edge/lab-10-enterprise-edge-default-routing.pkt)**
    * **What I did:** Established a strict border perimeter network connecting a multi-subnet corporate topology to an external simulated ISP endpoint over a public IP space (`203.0.113.0/30`).
    * **The Validation:** Configured and evaluated a candidate default path (`0.0.0.0/0`) to verify how the routing engine populates its "Gateway of Last Resort" flag.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R_CorpEdge# show ip route
        Gateway of last resort is 203.0.113.2 to network 0.0.0.0
        
        S* 0.0.0.0/0 [1/0] via 203.0.113.2
        ```
        </details>

---

### Part 4: Classless Subnetting Mastery & VLSM Design

* **[Lab 11: Fixed Class C Subdivision](./Part-4-Subnetting-and-VLSM/lab-11-fixed-class-c-subdivision.pkt)**
    * **What I did:** Partitioned a generic Class C block into 4 identical, structured blocks using the classic subnet formula to isolate broadcast boundaries for 45-device rooms.
    * **The Validation:** Verified successful cross-subnet transit across the newly structured `/26` segments and monitored local interface tracking.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R1# show ip route connected
             192.168.1.0/24 is variably subnetted, 4 subnets, 2 masks
        C       192.168.1.0/26 is directly connected, GigabitEthernet0/0
        C       192.168.1.64/26 is directly connected, GigabitEthernet0/1
        ```
        </details>

* **[Lab 12: High-Speed Arbitrary Subnet Mapping](./Part-4-Subnetting-and-VLSM/lab-12-block-size-trick-mapping.pkt)**
    * **What I did:** Practiced the real-world decimal **Block Size Trick** (`256 - Mask Value`) to find network boundaries instantly without tedious binary-to-decimal expansion.
    * **The Validation:** Verified that host `192.168.5.57/27` falls cleanly inside the native `192.168.5.32` subnet container block.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R1# show ip route connected
        C       192.168.5.32/27 is directly connected, GigabitEthernet0/0
        ```
        </details>

* **[Lab 13: Zero-Waste Router Interconnects](./Part-4-Subnetting-and-VLSM/lab-13-point-to-point-zero-waste.pkt)**
    * **What I did:** Optimized serial-equivalent link configurations between point-to-point routers by transitioning away from heavy subnet masks down to zero-overhead limits.
    * **The Validation:** Compared a traditional `/30` point-to-point allocation against a modern, zero-overhead `/31` mask config to prove the router handles directed links without network or broadcast ID padding.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R_Left# show ip interface brief | include Gigabit0/0
        GigabitEthernet0/0     10.10.10.0      YES manual up                    up
        
        R_Right# show ip interface brief | include Gigabit0/0
        GigabitEthernet0/0     10.10.10.1      YES manual up                    up
        ```
        </details>

* **[Lab 14: Class B Subnetting Matrix Optimization](./Part-4-Subnetting-and-VLSM/lab-14-class-b-matrix-optimization.pkt)**
    * **What I did:** Shifted subnetworking boundaries into a large Class B address environment, managing block size changes directly in the third octet.
    * **The Validation:** Isolated an arbitrary host block (`172.25.217.192/21`) and proved it resolves directly to the `172.25.216.0` prefix space.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R1# show ip route | include 172.25
        C       172.25.216.0/21 is directly connected, GigabitEthernet0/0
        ```
        </details>

* **[Lab 15: Class A Massive Scaling Scheme](./Part-4-Subnetting-and-VLSM/lab-15-class-a-massive-scaling.pkt)**
    * **What I did:** Borrowed 11 bits out of a default `/8` enterprise block to generate thousands of isolated corporate subnets with customizable host spaces.
    * **The Validation:** Tracked down the exact network parameters for a specific host IP (`10.217.182.223`) out of millions of possible combinations.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R1# show ip route | include 10.
        C       10.192.0.0/11 is directly connected, GigabitEthernet0/0
        ```
        </details>

* **[Lab 16: Chronological VLSM Network Engineering](./Part-4-Subnetting-and-VLSM/lab-16-chronological-vlsm-design.pkt)**
    * **What I did:** Hard-enforced the golden rule of Variable-Length Subnet Masking: sorting and provisioning varied site requirements chronologically from the absolute largest host footprint down to the smallest link.
    * **The Validation:** Carved out Tokyo and Toronto offices cleanly out of a single `/24` space without overlapping a single bit boundary.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R_Tokyo# show ip route connected
        C       192.168.1.0/25 is directly connected, GigabitEthernet0/0   (Tokyo LAN A)
        C       192.168.1.224/28 is directly connected, GigabitEthernet0/1 (Tokyo LAN B)
        ```
        </details>

---

### Part 5: Virtual LAN Isolation & Layer 2 Segmentation

* **[Lab 17: Flat Network Broadcast Domain Audit](./Part-5-VLAN-and-Layer2-Segmentation/lab-17-flat-network-broadcast-audit.pkt)**
    * **What I did:** Analyzed a flat, unsegmented Layer 2 switch fabric where multiple departments shared the default factory configuration.
    * **The Validation:** Utilized Simulation Mode to watch a single broadcast frame clone itself and force every host NIC in the company to waste CPU resources processing a localized broadcast storm.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW1# show vlan brief
        VLAN Name                             Status    Ports
        ---- -------------------------------- --------- -------------------------------
        1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
        ```
        </details>

* **[Lab 18: Logical VLAN Database Creation](./Part-5-VLAN-and-Layer2-Segmentation/lab-18-logical-vlan-database-creation.pkt)**
    * **What I did:** Broke up the flat layout by creating custom logical database containers for Engineering, HR, and Sales inside the switch database.
    * **The Validation:** Verified database persistence and checked that named VLAN matrices are ready before connecting devices.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW1# show vlan brief
        VLAN Name                             Status    Ports
        ---- -------------------------------- --------- -------------------------------
        10   Engineering                      active    
        20   HumanResources                   active    
        30   Sales                            active    
        ```
        </details>

* **[Lab 19: Access Port VLAN Binding](./Part-5-VLAN-and-Layer2-Segmentation/lab-19-access-port-vlan-binding.pkt)**
    * **What I did:** Hardcoded individual physical switchports into single-VLAN access mode, removing them from the global default network.
    * **The Validation:** Audited the switch's hardware allocation map to prove that the interfaces have migrated into completely isolated forwarding pools.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW1# show vlan brief
        VLAN Name                             Status    Ports
        ---- -------------------------------- --------- -------------------------------
        10   Engineering                      active    Fa0/1
        20   HumanResources                   active    Fa0/2
        30   Sales                            active    Fa0/3
        ```
        </details>

* **[Lab 20: Layer 2 Broadcast Isolation Verification](./Part-5-VLAN-and-Layer2-Segmentation/lab-20-layer2-broadcast-isolation-test.pkt)**
    * **What I did:** Executed a full validation test across the newly segmented access layer to verify BUM (Broadcast, Unknown Unicast, Multicast) traffic constraints.
    * **The Validation:** Proved via Simulation Mode that a broadcast originating in VLAN 10 is completely blocked from bleeding into ports assigned to VLAN 20 or 30.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW1# show mac address-table vlan 10
                  Mac Address Table
        -------------------------------------------
        Vlan    Mac Address       Type        Ports
        ----    -----------       --------    -----
          10    000a.f311.a102    DYNAMIC     Fa0/1
        ```
        </details>

---

### Part 6: Advanced Trunking, Native Overheads & Router-on-a-Stick (RoAS)

* **[Lab 21: Explicit Dot1q Trunking & VLAN Matrix Filters](./Part-6-Trunking-and-InterVLAN-Routing/lab-21-explicit-vlan-trunk-filtering.pkt)**
    * **What I did:** Built a link between two switches. Hardcoded the port into trunk mode and modified the allowed VLAN trunking matrix to filter out unneeded data.
    * **The Validation:** Manually dropped VLAN 20 traffic from entering the inter-switch link, proving you can secure paths by controlling which VLAN IDs can pass over a trunk.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW1# show interfaces trunk
        Port        Mode         Encapsulation  Status        Native vlan
        Gi0/1       on           802.1q         trunking      1
        
        Port        Vlans allowed on trunk
        Gi0/1       10,30
        ```
        </details>

* **[Lab 22: Native VLAN Manipulation & Mismatch Analysis](./Part-6-Trunking-and-InterVLAN-Routing/lab-22-native-vlan-mismatch-analysis.pkt)**
    * **What I did:** Moved the untagged traffic pathway away from the default VLAN 1 down to a custom native VLAN 1001 ID, then intentionally mismatched the configuration between neighbors.
    * **The Validation:** Analyzed CDP error logs to see how native mismatches misdirect untagged frames, accidentally leaking traffic across separate broadcast domains.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW1# show interfaces trunk | include Native
        Gi0/1       on           802.1q         trunking      1001
        
        -- CDP Console Log Event Generated --
        %CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on GigabitEthernet0/1 (1001), with SW2 GigabitEthernet0/1 (1).
        ```
        </details>

* **[Lab 23: Router-on-a-Stick (RoAS) Logical Gateway Architecture](./Part-6-Trunking-and-InterVLAN-Routing/lab-23-router-on-a-stick-gateways.pkt)**
    * **What I did:** Connected a single physical 2911 router port to a trunking switch interface to handle inter-VLAN routing without using up multiple hardware interfaces.
    * **The Validation:** Split a physical interface into subinterfaces, hardcoded dot1q encapsulation tags on each, and successfully routed packets across separate networks.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R1# show ip interface brief | include .
        GigabitEthernet0/0.10  192.168.1.62    YES manual up                    up
        GigabitEthernet0/0.20  192.168.1.126   YES manual up                    up
        GigabitEthernet0/0.30  192.168.1.190   YES manual up                    up
        ```
        </details>

* **[Lab 24: Advanced Native Gateways on Logical Subinterfaces](./Part-6-Trunking-and-InterVLAN-Routing/lab-24-advanced-subinterface-native.pkt)**
    * **What I did:** Configured a router to process untagged native frames coming from a switch trunk, exploring two different configuration methods.
    * **The Validation:** Validated Method A (using the `native` keyword on a subinterface) and Method B (binding the IP directly to the physical interface) to see how the router strips 802.1Q tags for native traffic.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        R1# show running-config interface g0/0.10
        interface GigabitEthernet0/0.10
         encapsulation dot1Q 10 native
         ip address 192.168.1.62 255.255.255.192
        ```
        </details>

---

### Part 7: Multilayer Switching Fabric & Switch Virtual Interfaces (SVIs)

* **[Lab 25: Switch Virtual Interface (SVI) Fabric Core Design](./Part-7-Multilayer-Switching-and-SVIs/lab-25-switch-virtual-interface-svi.pkt)**
    * **What I did:** Turned on a multilayer switch's internal routing table (`ip routing`) to handle inter-VLAN routing inside the switch at wire speed, bypassing the single-link router bottleneck.
    * **The Validation:** Created logical Switch Virtual Interfaces (SVIs) and tracked down the exact physical requirements (like an active access port or trunk) needed to keep an SVI "up/up".
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW_Core# show ip interface brief | include Vlan
        Vlan10                 192.168.1.62    YES manual up                    up
        Vlan20                 192.168.1.126   YES manual up                    up
        ```
        </details>

* **[Lab 26: Routed Interface Ports & Default Static Gateways](./Part-7-Multilayer-Switching-and-SVIs/lab-26-routed-ports-static-uplinks.pkt)**
    * **What I did:** Converted a standard Layer 2 switchport into a Layer 3 routed port using the `no switchport` command, establishing a point-to-point connection to an edge router.
    * **The Validation:** Verified that the port's role switched from switching to routing, and pointed a default static route to the edge router to reach external networks.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW_Core# show interfaces status | include Gi0/1
        Port      Name               Status       Vlan       Duplex  Speed Type
        Gi0/1     Uplink_To_R1       connected    routed     full    1000BaseTX
        ```
        </details>

---

### Part 8: Spanning Tree Protocol (STP) Calculations & Advanced Toolkit Optimization

* **[Lab 27: Spanning Tree Baseline Calculations & Root Election](./Part-8-Spanning-Tree-Protocol-and-Toolkit/lab-27-stp-root-bridge-elections.pkt)**
    * **What I did:** Wired three switches in a redundant triangle loop and analyzed how Spanning Tree calculates a loop-free path out of the box.
    * **The Validation:** Logged system parameters to analyze Root Bridge election variables (Priority + MAC address tie-breakers) and mapped out exactly which non-root port gets blocked.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW3# show spanning-tree brief
        VLAN0001
          Root ID      Priority    32769
                       Address     0001.c9a1.a105
                       Cost        4
                       Port        Gi0/1 (GigabitEthernet0/1)
        
        Interface        Role Stet Cost      Prio.Nbr Type
        ---------------- ---- ---- --------- -------- ------------------------
        Gi0/1            Root FWD  4         128.1    Shr
        Gi0/2            Altn BLK  4         128.2    Shr
        ```
        </details>

* **[Lab 28: Manual Topology Tuning & Per-VLAN Load Balancing](./Part-8-Spanning-Tree-Protocol-and-Toolkit/lab-28-manual-stp-load-balancing.pkt)**
    * **What I did:** Overrode the default Spanning Tree values using the mandatory `4096` priority increments to configure custom root choices per VLAN.
    * **The Validation:** Configured SW1 as primary root for VLAN 10 and SW2 as primary root for VLAN 20, keeping both links active by load balancing different VLANs across different paths.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW1# show spanning-tree vlan 10 | include root
        This bridge is the root
        
        SW1# show spanning-tree vlan 20 | include root
        Root ID      Priority    24596
                     Address     0002.b4a2.c202
        ```
        </details>

* **[Lab 29: PortFast Edge Transitions & BPDU Guard Protection](./Part-8-Spanning-Tree-Protocol-and-Toolkit/lab-29-portfast-edge-bpdu-guard.pkt)**
    * **What I did:** Configured PortFast on user access ports to bypass the 30-second listening/learning delay, and locked them down with BPDU Guard to protect against unauthorized switches.
    * **The Validation:** Connected a rogue switch to a port to trigger BPDU Guard, verifying that the port immediately shuts down and drops into an `err-disabled` state to protect the network.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW1# show interfaces fa0/1 status
        Port      Name               Status       Vlan       Duplex  Speed Type
        Fa0/1     User_Workstation   err-disabled 10         full    100   100BaseTX
        ```
        </details>

* **[Lab 30: Unidirectional Loop Defense via Loop Guard](./Part-8-Spanning-Tree-Protocol-and-Toolkit/lab-30-loop-guard-unidirectional.pkt)**
    * **What I did:** Configured Loop Guard on root and non-designated ports to defend the backbone against physical link failures (like a fiber rx drop) that cause silent loops.
    * **The Validation:** Monitored the control plane to ensure that if BPDUs unexpectedly stop arriving, the port safely locks up in a `loop-inconsistent` broken state instead of opening up and creating a loop.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>

        ```text
        SW3# show spanning-tree inconsistentports
        Name                 Interface            Inconsistency
        -------------------- -------------------- ------------------
        VLAN0001             GigabitEthernet0/2   Loop Inconsistent
        
        Number of inconsistent ports: 1
        ```
        </details>

---

### Part 9: Fast Convergence & Link Aggregation
 
* **[Lab 31: Rapid PVST+ Migration & Edge Optimization](./Part-9-Fast-Convergence-and-Link-Aggregation/lab-31-rapid-pvst-edge-tuning.pkt)**
    * **What I did:** Migrated a three-switch redundant triangle from legacy 802.1D to Rapid PVST+ (802.1w). Configured root primary/secondary roles across VLANs 10 and 20, forced user-facing interfaces to operate as PortFast edge links, and hardcoded switch-to-switch links to `point-to-point`.
    * **The Validation:** Simulated an active root link failure to prove sub-second convergence, verifying that Alternate ports immediately take over forwarding roles without undergoing classic 30–50 second listening/learning delays.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>
       
        ```text
        SW1# show spanning-tree vlan 10
        VLAN0010
          Spanning tree enabled protocol rstp
          Root ID      Priority    24586
                       Address     0001.C710.A101
                       This bridge is the root
                       Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
 
        Interface        Role Sts Cost      Prio.Nbr Type
        ---------------- ---- ---- --------- -------- --------------------------------
        Gi0/1            Desg FWD  4         128.1    P2p
        Gi0/2            Desg FWD  4         128.2    P2p
        Fa0/1            Desg FWD  19        128.3    Edge P2p
        ```
        </details>
 
* **[Lab 32: Multi-Vendor LACP EtherChannel Aggregation](./Part-9-Fast-Convergence-and-Link-Aggregation/lab-32-lacp-etherchannel-aggregation.pkt)**
    * **What I did:** Resolved access-to-distribution bandwidth oversubscription by bundling 4 parallel physical FastEthernet links into a single logical IEEE 802.3ad Port-Channel. Configured `active` mode on the access side, `passive` mode on the distribution side, and modified the global load-balancing algorithm to evaluate `src-dst-mac` hashes.
    * **The Validation:** Verified that Spanning Tree treats the entire 4-link bundle as one single logical interface (`Po1`), preventing port blocking while aggregating throughput and ensuring sub-second hardware failover if an individual link drops.
    * <details>
        <summary> Click to view Cisco IOS Verification Proof</summary>  
       
        ```text
        SW_Access# show etherchannel summary
        Flags:  D - down        P - bundled in port-channel
                I - stand-alone s - suspended
                H - Hot-standby (LACP only)
                R - Layer3      S - Layer2
                U - in use      N - allocated to Port-channel
 
        Group Port-Channel Protocol    Ports
        ------+-------------+-----------+-----------------------------------------------
        1     Po1(SU)        LACP        Fa0/1(P) Fa0/2(P) Fa0/3(P) Fa0/4(P)
 
        SW_Access# show port-channel load-balance
        Source & Destination MAC Address
        ```
        </details>
 
---
