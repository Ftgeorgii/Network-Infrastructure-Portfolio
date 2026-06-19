# Cisco Networking Labs

Hands-on Cisco IOS labs built in Packet Tracer during network engineering training at Vatanix Technologies, Trichy.

Each lab includes a full README with topology diagram, step-by-step configuration with explanations, verification commands, and the Packet Tracer `.pkt` file.

---

## Lab Index

| # | Lab | Topics Covered | Tool | Status |
|---|-----|----------------|------|--------|
| 01 | [Basic Switch Config · VLANs · SSH](01_Basic_Switch_VLAN_SSH/) | Hostname · Enable Secret · Password Encryption · VLANs · SVIs · Telnet · SSH v2 · Port Security | Packet Tracer | ✅ Complete |
| 02 | [Spanning Tree Protocol](02_Spanning_Tree_Protocol/) | Root Bridge Election · Root/Designated/Alternate Ports · Failover · PortFast · BPDU Guard | Packet Tracer | ✅ Complete |
| 03 | Inter-VLAN Routing — SVI | Layer-3 Switch · SVI · Trunking · ip routing | Packet Tracer | 🔄 Planned |
| 04 | Inter-VLAN Routing — Router-on-a-Stick | Subinterfaces · 802.1Q Encapsulation | Packet Tracer | 🔄 Planned |
| 05 | OSPF Single Area | OSPF Process · DR/BDR Election · `show ip ospf neighbor` | Packet Tracer | 🔄 Planned |
| 06 | Access Control Lists | Standard ACLs · Extended ACLs · `show access-lists` | Packet Tracer | 🔄 Planned |

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

## Lab Format

Each completed lab folder contains:

- `README.md` — objective, topology, device connections, IP addressing, key concepts, step-by-step configuration with explanations, verification commands, troubleshooting scenario, lessons learned
- `*.pkt` — the Packet Tracer file
- `screenshots/` — topology diagrams and CLI verification output

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

# --- SVI — Management IP ---
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown

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
