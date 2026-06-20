# Cisco Networking Labs

Hands-on Cisco IOS labs built in Packet Tracer during network engineering training at Vatanix Technologies, Trichy.

Each lab includes a full README with topology diagram, step-by-step configuration with explanations, verification commands, `show running-config` reference, and the Packet Tracer `.pkt` file.

---

## Lab Index

| # | Lab | Topics Covered | Tool | Status |
|---|-----|----------------|------|--------|
| 01 | [Basic Switch Config · VLANs · SSH](01_Basic_Switch_VLAN_SSH/) | Hostname · Enable Secret · Password Encryption · VLANs · SVIs · Telnet · SSH v2 · Port Security | Packet Tracer | ✅ Complete |
| 02 | [Spanning Tree Protocol](02_Spanning_Tree_Protocol/) | Root Bridge Election · Root/Designated/Alternate Ports · Failover · PortFast · BPDU Guard | Packet Tracer | ✅ Complete |
| 03 | [Inter-VLAN Routing — SVI](03_Inter_VLAN_Routing_SVI/) | Layer-3 Switch · SVI · Trunking · ip routing · Multi-switch routing | Packet Tracer | ✅ Complete |
| 04 | [Inter-VLAN Routing — Router-on-a-Stick](04_Inter_VLAN_Routing_ROS/) | Subinterfaces · 802.1Q Encapsulation · ROAS vs SVI | Packet Tracer | ✅ Complete |
| 05 | [Static Routing](05_Static_Routing/) | Standard Static Routing · Default Static Routing · Wrong Subnet Mask Troubleshooting | Packet Tracer | 🔄 In Progress |
| 06 | OSPF Single Area | OSPF Process · DR/BDR Election · `show ip ospf neighbor` | Packet Tracer | 🔄 Planned |
| 07 | Access Control Lists | Standard ACLs · Extended ACLs · `show access-lists` | Packet Tracer | 🔄 Planned |

---

## Lab 01 — Basic Switch Configuration, VLANs, Telnet & SSH

**[→ Full Lab Documentation](01_Basic_Switch_VLAN_SSH/README.md)**

A Cisco switch configured from scratch with two VLANs, management access via SVI, and secure remote access over SSH — replacing Telnet as a baseline test.

**Key concepts:** `enable secret` vs `enable password` · SVI · `switchport mode access` and DTP · cross-VLAN routing limitations · Telnet vs SSH v2 · RSA key generation

---

## Lab 02 — Spanning Tree Protocol

**[→ Full Lab Documentation](02_Spanning_Tree_Protocol/README.md)**

Three-switch triangle topology demonstrating automatic loop prevention. Root Bridge forced via priority, port roles verified across all three switches, and live failover tested by shutting the primary uplink.

**Key concepts:** Root Bridge election · Root Port vs Designated Port vs Alternate (Blocking) Port · BLK→FWD failover and cost recalculation · PortFast and BPDU Guard on access ports

---

## Lab 03 — Inter-VLAN Routing Using SVI

**[→ Full Lab Documentation](03_Inter_VLAN_Routing_SVI/README.md)**

Two Layer-3 switches (MLS1, MLS2) connected via trunk, with MLS1 holding SVIs for all three VLANs and acting as the central routing hub. Includes a troubleshooting scenario for a missing VLAN entry breaking cross-switch routing.

**Key concepts:** SVI (Switched Virtual Interface) · `ip routing` on a Layer-3 switch · trunk-carried VLANs that exist on both switches · centralised vs distributed SVI ownership

---

## Lab 04 — Inter-VLAN Routing Using Router-on-a-Stick

**[→ Full Lab Documentation](04_Inter_VLAN_Routing_ROS/README.md)**

Single router (2911) connected to a Layer-2 switch (2960-24TT) via one trunk link. Three router subinterfaces handle routing for three VLANs using 802.1Q encapsulation.

**Key concepts:** Router subinterfaces · `encapsulation dot1Q` · why the physical interface carries no IP · ROAS vs SVI performance and cost tradeoffs

---

## Lab 05 — Static Routing

**[→ Folder Overview](05_Static_Routing/README.md)**

Covers both standard and default static routing approaches across separate subfolders.

**[01 — Standard Static Routing](05_Static_Routing/01_Standard_Static_Routing/README.md)** ✅ Complete
Three-router topology with explicit `ip route` entries for every network. Includes a deliberate wrong-subnet-mask fault and the full troubleshooting process — diagnosing a `/30` route that should have been `/24`, isolating the fault to the return path on the destination router, and fixing it live.

**02 — Default Static Routing** 🔄 In Progress
Single default route (`ip route 0.0.0.0 0.0.0.0`) on a stub/edge router instead of per-network static routes.

**Key concepts:** Static route syntax and next-hop selection · `show ip route` route codes (C, L, S) · tracert hop-by-hop path verification · standard vs default routing tradeoffs

---

## Lab Format

Each completed lab folder contains:

- `README.md` — objective, topology, device connections, IP addressing, key concepts, step-by-step configuration with explanations, verification commands, troubleshooting scenario, lessons learned
- `*.pkt` — the Packet Tracer file
- `screenshots/` — topology diagrams and CLI verification output (running-configs, routing tables, ping/tracert results)
- `show-running-config-*.html` — full device configuration reference with syntax highlighting (where applicable)

---

## Essential Cisco IOS Commands Reference

```
# --- Show Commands ---
show version                    # IOS version, uptime, hardware
show running-config             # Current active configuration
show ip interface brief         # Interface status and IPs
show vlan brief                 # VLAN list and assigned ports
show interfaces trunk           # Trunk port status
show ip route                   # Routing table
show ip ospf neighbor           # OSPF neighbour relationships
show ip ssh                     # SSH version and status
show access-lists               # ACL entries and hit counts
show spanning-tree              # STP port states and root bridge

# --- Basic Setup ---
enable
configure terminal
hostname SW1
no ip domain-lookup              # Prevent DNS lookup on mistyped commands
service password-encryption      # Encrypt plain-text passwords in config

# --- VLAN Config (Switch) ---
vlan 10
 name SALES
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/5
 switchport mode trunk

# --- SVI — Management / Routing IP ---
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown
ip routing                       # Required to route between SVIs

# --- Router-on-a-Stick Subinterface ---
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

# --- Static Routing ---
ip route [destination-network] [subnet-mask] [next-hop-IP]
ip route 0.0.0.0 0.0.0.0 [next-hop-IP]    # Default route

# --- SSH Setup ---
ip domain-name lab.local
username admin secret cisco123
crypto key generate rsa          # 1024 (Packet Tracer) / 2048+ (production)
ip ssh version 2
line vty 0 15
 login local
 transport input ssh

# --- Spanning Tree ---
spanning-tree vlan 1 root primary
interface fa0/3
 spanning-tree portfast
 spanning-tree bpduguard enable

# --- OSPF ---
router ospf 1
 router-id 1.1.1.1
 network 192.168.1.0 0.0.0.255 area 0

# --- Extended ACL ---
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
access-list 100 deny ip any any
interface GigabitEthernet0/0
 ip access-group 100 in
```

---

> Labs are added progressively as training advances. Each lab builds on concepts from the previous one.
