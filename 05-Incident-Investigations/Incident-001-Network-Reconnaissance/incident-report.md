# Incident 001 — Network Reconnaissance

## Incident Overview

A controlled network reconnaissance activity was performed against the
Windows endpoint in the isolated SOC home lab.

The purpose of this activity was to simulate an attacker performing
initial network reconnaissance and to establish baseline evidence that
can later be detected and investigated using network monitoring and
SIEM technologies.

---

## Environment

| Component | Role | IP Address |
|---|---|---|
| Kali Linux | Simulated Attacker | 192.168.56.102 |
| Windows | Target Endpoint | 192.168.56.101 |
| VirtualBox | Hypervisor | Host-Only Network |

Network:

`192.168.56.0/24`

---

## Attack Technique

**Technique:** Network Service Scanning

**Tool:** Nmap

**Attack Source:** 192.168.56.102

**Target:** 192.168.56.101

---

## Command Executed

```bash
nmap 192.168.56.101