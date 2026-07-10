# VPN Connectivity Incident — AD Account Lockout

This project documents the end-to-end incident response workflow 
for a VPN authentication failure caused by an Active Directory 
account lockout in an enterprise environment.

---

## Problem Statement

A remote employee reported being unable to connect to the corporate 
VPN despite using correct credentials. The error message returned 
was "The connection was closed by the remote computer." The employee 
had no access to internal resources or file shares, creating a 
business-critical P1 incident requiring immediate resolution.

The core challenge: the VPN service was fully operational, the 
network was reachable, and the user believed their credentials 
were correct — making the root cause non-obvious and requiring 
systematic investigation across the Identity, Policy, and 
Application layers.

---

## Solution Overview

Built a self-hosted enterprise VPN authentication stack using 
Windows Server 2022 with Active Directory Domain Services, RRAS 
(Routing and Remote Access), and NPS (Network Policy Server) as 
the RADIUS authentication backend. Deployed osTicket via XAMPP 
as the ITSM ticketing platform to manage the full incident lifecycle.

Engineered an AD account lockout scenario by configuring a domain 
lockout policy (3 failed attempts) and triggering it through 
repeated failed VPN login attempts. Worked the incident from 
ticket creation through root cause identification and resolution 
using the CompTIA A+ 6-step troubleshooting methodology, 
documenting every step as internal notes in osTicket the way a 
real help desk technician would.

Root cause identified: Active Directory account lockout triggered 
by repeated failed VPN authentication attempts. Resolution: account 
unlocked via Active Directory Users and Computers, user educated 
on lockout policy, preventive measures documented.

---

## Video Walkthrough

[Coming Soon](#)

---

## Tools Used

- Windows Server 2022
- Active Directory Domain Services (AD DS)
- Routing and Remote Access (RRAS)
- Network Policy Server (NPS)
- osTicket (self-hosted via XAMPP)
- Group Policy Management
- Event Viewer
- VirtualBox
- Windows 11 Pro (VPN client)
- GitHub

---

## Project Timeline

- **Day 1:** Installed and configured RRAS and NPS roles on 
  existing domain controller. Resolved service startup conflicts 
  caused by co-located RRAS/NPS architecture on Windows Server 
  2022. Established baseline VPN connectivity from Windows 11 
  client.
- **Day 2:** Deployed osTicket via XAMPP. Resolved Apache port 
  conflicts with IIS. Configured MySQL database, completed 
  osTicket installation, and verified admin panel access.
- **Day 3:** Created test user Alie Layus (alayus). Configured 
  domain account lockout policy via Group Policy (3 failed 
  attempts). Engineered account lockout scenario through repeated 
  failed VPN login attempts. Created incident ticket #676210 in 
  osTicket.
- **Day 4:** Worked the full incident using CompTIA A+ 6-step 
  troubleshooting methodology. Documented all steps as internal 
  notes in osTicket. Unlocked AD account, verified VPN 
  connectivity restored, communicated resolution to user, 
  and closed ticket as Resolved.
- **Day 5:** GitHub documentation, README writeup, and video 
  walkthrough recording.

---

## Key Accomplishments

- Built a functional enterprise VPN authentication stack from 
  scratch using RRAS, NPS, and Active Directory on Windows 
  Server 2022
- Deployed and configured a self-hosted osTicket ITSM instance 
  using XAMPP on Windows Server
- Demonstrated full ticket lifecycle management from incident 
  creation through resolution and closure
- Applied CompTIA A+ 6-step troubleshooting methodology to a 
  real incident scenario, documenting each step the way a 
  working technician would
- Identified root cause by cross-referencing Active Directory 
  account status and Event Viewer logs simultaneously
- Demonstrated the ability to adapt when a planned scenario 
  didn't work as expected due to architectural constraints — 
  pivoting to a more realistic and equally valid incident

---

## Engineering Notes

### Original Design Goal
Simulate an NPS shared secret mismatch between RRAS and a 
separate NPS server — a common enterprise VPN misconfiguration.

### Discovery
Because RRAS, NPS, and Active Directory were co-located on 
the same Windows Server 2022 instance, Windows internally 
authenticated requests instead of exercising a true 
network-based RADIUS exchange. As a result, changing the 
shared secret, disabling NPS policies, and stopping the NPS 
service did not produce the expected authentication failure.

This is a real architectural behavior specific to Windows 
Server 2022 when these roles share the same host — in a 
production enterprise, RRAS and NPS would typically run on 
separate servers, which is where shared secret mismatches 
become a genuine failure vector.

### Project Adjustment
The incident was redesigned around an Active Directory account 
lockout — one of the most common real-world causes of VPN 
authentication failures. This scenario better fits a 
single-server Windows lab while still demonstrating enterprise 
troubleshooting methodology across the Identity, Policy, and 
Application layers.

---

## What I Learned

- How RRAS and NPS work together to create an enterprise VPN 
  authentication stack, and how Windows Server 2022 handles 
  co-located role communication differently than a distributed 
  architecture
- The importance of checking the Identity layer first when VPN 
  authentication fails — the problem isn't always the VPN
- How to use Event Viewer and Active Directory simultaneously 
  to cross-reference evidence during an investigation
- How to document a real incident in osTicket the way a 
  working help desk technician would — including dead ends, 
  not just the final answer
- That architectural limitations are not failures — 
  understanding why something doesn't work and adapting 
  intelligently is a core professional skill
- How to apply the CompTIA A+ 6-step troubleshooting 
  methodology to a real scenario rather than just memorizing 
  it for an exam
