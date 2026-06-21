# Troubleshooting Cases

Structured incident case studies documenting real faults encountered, diagnosed,
and resolved across the Windows Server and Cisco Networking labs in this
portfolio. Each case follows a standard NOC-style incident format: symptom,
diagnosis, root cause, fix, verification, and lessons learned.

This folder demonstrates structured, layer-by-layer troubleshooting methodology —
the core skill expected in NOC/L1 network engineer roles.

---

## Case Index

| # | Incident | Severity | System | Status |
|---|----------|----------|--------|--------|
| [INC-001](INC-001_DNS_Reverse_Lookup_Failure.md) | DNS Reverse Lookup Failure — Missing PTR Record | P3 | Windows Server DNS | ✅ Resolved |
| [INC-002](INC-002_DHCP_APIPA_Authorization_Failure.md) | Client Receiving APIPA Address — DHCP Not Authorized | P2 | Windows Server DHCP | ✅ Resolved |
| [INC-003](INC-003_VLAN_Missing_From_Trunk_Database.md) | Cross-Switch VLAN Unreachable — Missing VLAN in Database | P2 | Cisco Multilayer Switch (SVI) | ✅ Resolved |
| [INC-004](INC-004_Wrong_Subnet_Mask_Static_Route.md) | One-Way Connectivity Failure — Wrong Subnet Mask on Static Route | P2 | Cisco Router (Static Routing) | ✅ Resolved |
| [INC-005](INC-005_Port_Err_Disabled_BPDU_Guard.md) | Access Port Err-Disabled — BPDU Guard Triggered | P3 | Cisco Switch (STP) | ✅ Resolved |

---

## Quick Reference

**[→ Common Troubleshooting Scenarios](Common_Troubleshooting_Scenarios.md)**

A fast-lookup table covering common faults across the entire training journey —
hardware, BIOS/UEFI, OS/boot issues, networking fundamentals, email/auth,
printers/firewall, virtualization, server hardware, RAID, Active Directory/GPO,
and Cisco switching/routing. Organized as Symptom → Likely Cause → Fix →
Verification Command for quick recall and interview preparation. The detailed
case studies above go deep on five specific incidents; this document covers
breadth across everything from Day 1 onward.

---

## Methodology Used Across All Cases

1. **Confirm the symptom precisely** — reproduce the exact failure before assuming a cause
2. **Isolate by layer** — work through OSI layers systematically (physical → data link → network → transport) rather than guessing
3. **Check the obvious first** — service status, interface status, basic reachability — before deep configuration review
4. **Compare expected vs actual output** — `show` command output against what the configuration *should* produce
5. **Identify root cause, not just the symptom** — distinguish "what failed" from "why it failed"
6. **Fix, then verify** — every fix in this folder is followed by explicit verification, not assumed to have worked
7. **Document the lesson** — every case ends with what this incident teaches for future deployments

---

## Severity Definitions Used

| Severity | Definition |
|----------|------------|
| P1 | Full outage — multiple systems or users affected, no workaround |
| P2 | Significant impact — single system or service down, or one-way/partial failure |
| P3 | Minor impact — single port/record/non-critical service, workaround available or limited scope |

---

## Source Labs

These incidents are drawn from real configuration work in:

- [Windows_Server/03_DNS_Configuration](../Windows_Server/03_DNS_Configuration/) — INC-001
- [Windows_Server/04_DHCP_Configuration](../Windows_Server/04_DHCP_Configuration/) — INC-002
- [Cisco_Labs/03_Inter_VLAN_Routing_SVI](../Cisco_Labs/03_Inter_VLAN_Routing_SVI/) — INC-003
- [Cisco_Labs/05_Static_Routing](../Cisco_Labs/05_Static_Routing/) — INC-004
- [Cisco_Labs/02_Spanning_Tree_Protocol](../Cisco_Labs/02_Spanning_Tree_Protocol/) — INC-005

---

> Case studies are added as new troubleshooting scenarios are encountered during training.
