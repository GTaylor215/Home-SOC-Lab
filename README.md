# Home SOC Lab

A hands-on Security Operations Center (SOC) home lab designed to
simulate real-world security monitoring, threat detection, network
analysis, and incident response workflows.

The project uses an isolated virtual environment to simulate attacker
and victim activity and progressively integrates security monitoring
and detection technologies.


---
## Project Objectives

- Build an isolated SOC environment using virtualization
- Configure an attacker and victim endpoint
- Perform controlled network reconnaissance
- Capture and analyze network traffic
- Deploy SIEM capabilities
- Implement IDS/IPS monitoring
- Collect and analyze endpoint telemetry
- Practice threat detection and investigation
- Develop incident response workflows
- Explore security automation and SOAR concepts


---
## Lab Architecture

The lab is built using VirtualBox and an isolated Host-Only network.

```text
                         VirtualBox
                             │
                  Host-Only SOC Network
                    192.168.56.0/24
                             │
                ┌────────────┴────────────┐
                │                         │
           🐉 Kali Linux              🪟 Windows
           Attacker VM               Victim VM
          192.168.56.102            192.168.56.101
                │                         │
                └────────────┬────────────┘
                             │
                    Security Monitoring
                             │
              ┌──────────────┼──────────────┐
              │              │              │
           Wireshark      IDS/IPS         SIEM
                             │              │
                         Suricata         Wazuh





---
## Lab Environment

| Component | Role |
|---|---|
| VirtualBox | Hypervisor |
| Kali Linux | Simulated attacker |
| Windows | Victim endpoint |
| Host-Only Network | Isolated SOC network |
| Nmap | Network reconnaissance |
| Wireshark | Network traffic analysis |
| Suricata | IDS/IPS |
| Wazuh | SIEM / security monitoring |
| Sysmon | Windows endpoint telemetry |
| SOAR | Security automation and response |



---
## Network Configuration

| Device | IP Address | Role |
|---|---:|---|
| VirtualBox Host Adapter | 192.168.56.1 | Host |
| SOC-Windows | 192.168.56.101 | Victim |
| SOC-Kali | 192.168.56.102 | Attacker |

Network: `192.168.56.0/24`

The Host-Only network provides an isolated environment for controlled
security testing between the virtual machines.



---
## Current Investigations

### Incident 001 — Network Reconnaissance

**Status:** Open

A controlled Nmap reconnaissance scan was performed from Kali Linux
against the Windows endpoint.

**Source:**

`192.168.56.102`

**Target:**

`192.168.56.101`

**Command:**

```bash
nmap 192.168.56.101

Initial Result

The Windows endpoint was identified as reachable, while all 1,000
scanned TCP ports were reported as filtered.

The activity will be used as a baseline event for future detection
and investigation using network monitoring and SIEM technologies.




---
## Technologies:

### Virtualization

- VirtualBox

### Operating Systems

- Kali Linux
- Windows

### Network Analysis

- Wireshark
- Nmap

### Detection & Monitoring

- Wazuh
- Sysmon
- Suricata

### Security Operations

- SIEM
- IDS/IPS
- SOAR
- Incident Response



---
## Project Progress:

### Completed

- VirtualBox environment configured
- Windows VM deployed
- Kali Linux VM deployed
- Host-Only SOC network configured
- Windows/Kali IP addressing configured
- Network connectivity tested
- Nmap installed
- Initial reconnaissance activity performed
- First incident documented

### In Progress / Planned

- Wireshark deployment
- Network traffic capture and analysis
- Suricata IDS/IPS deployment
- IDS detection configuration
- Wazuh SIEM deployment
- Windows endpoint telemetry integration
- SIEM alert investigation
- Incident response workflow development
- SOAR automation
- Additional security investigation scenarios




---
## Skills Demonstrated

- SOC operations
- Network reconnaissance
- Network traffic analysis
- SIEM monitoring
- IDS/IPS detection
- Endpoint telemetry
- Threat detection
- Security alert investigation
- Incident response
- Security automation
- Log analysis
- MITRE ATT&CK mapping
- Virtual machine and network configuration
- Security documentation
- Technical troubleshooting



---
## Documentation

Detailed documentation is maintained throughout the project, including
lab configuration, network setup, security tool deployment,
investigation reports, evidence, and lessons learned.


## Disclaimer

All security testing performed in this project is conducted within an
isolated, self-contained virtual laboratory environment for educational
and defensive security purposes.
