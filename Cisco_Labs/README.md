# Cisco Networking Labs

Hands-on Cisco IOS labs built in Packet Tracer during network engineering training at Vatanix Technologies, Trichy.

Each lab includes a full README with topology diagram, step-by-step configuration with explanations, verification commands, and a Packet Tracer `.pkt` file.

---

## Lab Index

| # | Lab | Topics Covered | Tool | Status |
|---|-----|----------------|------|--------|
| 01 | [Basic Switch Config · VLANs · SSH](01_Basic_Switch_VLAN_SSH/) | Hostname · Enable Secret · Password Encryption · VLANs · SVIs · Telnet · SSH v2 · Port Security | Packet Tracer | ✅ Complete |
| 02 | Inter-VLAN Routing | Router-on-a-Stick · Subinterfaces · `show ip route` | Packet Tracer | 🔄 Planned |
| 03 | OSPF Single Area | OSPF Process · DR/BDR Election · `show ip ospf neighbor` | Packet Tracer | 🔄 Planned |
| 04 | Spanning Tree Protocol | Root Bridge Election · Port States · BPDU | Packet Tracer | 🔄 Planned |
| 05 | Access Control Lists | Standard ACLs · Extended ACLs · `show access-lists` | Packet Tracer | 🔄 Planned |

---

## Lab 01 — Basic Switch Configuration, VLANs, Telnet & SSH

**[→ Full Lab Documentation](01_Basic_Switch_VLAN_SSH/README.md)**

![Lab 01 Topology](01_Basic_Switch_VLAN_SSH/screenshots/topology.png)

### What Was Built

A Cisco switch configured from scratch with two VLANs, management access via SVI, and secure remote access over SSH — replacing Telnet as a baseline test.

| Device | Interface | IP Address | VLAN | Role |
|--------|-----------|------------|------|------|
| SW1 | VLAN 10 | 192.168.10.1/24 | SALES | SVI / Management |
| SW1 | VLAN 20 | 192.168.20.1/24 | HR | SVI / Management |
| PC0 | Fa0/1 | 192.168.10.10/24 | VLAN 10 | End Device |
| PC3 | Fa0/2 | 192.168.10.20/24 | VLAN 10 | End Device |
| PC1 | Fa0/3 | 192.168.20.10/24 | VLAN 20 | End Device |
| PC2 | Fa0/4 | 192.168.20.20/24 | VLAN 20 | End Device |

### Key Concepts Practised

- `enable secret` vs `enable password` — MD5 hashing vs plain-text storage
- `service password-encryption` — Type 7 baseline hardening
- SVI (Switched Virtual Interface) — Layer 3 management IP on a Layer 2 switch
- `switchport mode access` — why it is always set explicitly (disables DTP)
- Why cross-VLAN pings fail without a Layer 3 routing device
- Telnet vs SSH — clear-text vs encrypted session, protocol difference
- `login` vs `login local` — shared line password vs local user database
- RSA key generation and `ip ssh version 2` — mandatory for SSH v2

### Files

| File | Description |
|------|-------------|
| [`README.md`](01_Basic_Switch_VLAN_SSH/README.md) | Full lab documentation — 19 steps with explanations |
| [`Basic_Switch_VLAN_SSH.pkt`](01_Basic_Switch_VLAN_SSH/Basic_Switch_VLAN_SSH.pkt) | Packet Tracer simulation file |
| [`screenshots/topology.png`](01_Basic_Switch_VLAN_SSH/screenshots/topology.png) | Network topology diagram |

---

## Cisco IOS Quick Reference

```
# Show Commands
show version                     # IOS version, uptime, hardware
show running-config              # Full current configuration
show ip interface brief          # Interface status and IPs at a glance
show vlan brief                  # VLAN list and assigned ports
show interfaces trunk            # Trunk port status
show ip route                    # Routing table
show ip ospf neighbor            # OSPF neighbour adjacencies
show ip ssh                      # SSH version and status
show access-lists                # ACL entries and hit counts
show spanning-tree               # STP port states and root bridge

# Basic Setup
enable
configure terminal
hostname SW1
no ip domain-lookup              # Prevent DNS lookup on mistyped commands
service password-encryption      # Encrypt plain-text passwords in config

# VLAN Configuration (Switch)
vlan 10
 name SALES
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10

# SVI — Management IP
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown

# SSH Setup
ip domain-name lab.local
username admin secret cisco123
crypto key generate rsa          # Enter 1024 (Packet Tracer) / 2048 (production)
ip ssh version 2
line vty 0 15
 login local
 transport input ssh

# OSPF
router ospf 1
 router-id 1.1.1.1
 network 192.168.1.0 0.0.0.255 area 0

# Extended ACL
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
access-list 100 deny ip any any
interface GigabitEthernet0/0
 ip access-group 100 in
```

---

> Labs are added progressively as training advances. Each lab builds on the previous one.
