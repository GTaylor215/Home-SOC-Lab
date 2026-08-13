# Host-Only Network Configuration

## Purpose

The SOC lab uses a VirtualBox Host-Only network to isolate security
testing traffic from the physical network.

## Network Configuration

| Device | Role | IP Address |
|---|---|---|
| VirtualBox Host Adapter | Host | 192.168.56.1 |
| SOC-Windows | Victim Endpoint | 192.168.56.101 |
| SOC-Kali | Attacker | 192.168.56.102 |

## Network

- Network: 192.168.56.0/24
- Subnet Mask: 255.255.255.0
- Network Type: VirtualBox Host-Only

## Purpose of the Architecture

Kali Linux will generate controlled security activity against the
Windows endpoint.

The isolated network allows reconnaissance, packet analysis, threat
detection, and incident response activities to be performed without
targeting devices on the physical network.

## Current Status

- [x] Host-Only network created
- [x] Windows connected to Host-Only network
- [x] Kali connected to Host-Only network
- [x] Windows assigned 192.168.56.101
- [x] Kali assigned 192.168.56.102
- [x] Windows-to-Kali connectivity verified
- [] Kali-to-Windows connectivity verified at the network layer

## Notes

Windows Firewall prevented ICMP echo requests from receiving a response
when testing from Kali. Windows was able to successfully ping Kali,
confirming that the virtual network itself was operational.