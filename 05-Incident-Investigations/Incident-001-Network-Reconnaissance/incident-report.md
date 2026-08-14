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



---

## Network Traffic Analysis

Wireshark was used to capture and analyze the network traffic generated
during the controlled Nmap reconnaissance scan.

### Capture Details

Capture Interface: eth0

Source: 192.168.56.102 (Kali Linux)

Destination: 192.168.56.101 (Windows)

Protocol: TCP

Wireshark Display Filter:

ip.addr == 192.168.56.101 && tcp

### Observations

The packet capture showed numerous TCP SYN packets originating from the
Kali Linux system and targeting multiple TCP ports on the Windows
endpoint.

Observed destination ports included ports such as 22, 25, 110, 135,
139, 443, 445, 554, 993, 995, 1723, 3306, 3389, 5900, and 8888.

The repeated TCP SYN packets directed toward numerous destination ports
are consistent with network port-scanning behavior.

### Analysis

The Wireshark capture provided packet-level evidence of the
reconnaissance activity previously observed through Nmap.

The Kali Linux host at 192.168.56.102 generated TCP SYN probes against
multiple ports on the Windows endpoint at 192.168.56.101.

Many connection attempts did not progress into completed TCP
handshakes. This behavior is consistent with the earlier Nmap scan,
which reported the scanned TCP ports as filtered with no response.

By examining the packet capture, the reconnaissance activity could be
identified independently of the Nmap output based on the communication
pattern between the source and destination systems.

### Detection Indicators

Indicators observed during the reconnaissance activity included:

- A single source IP communicating with numerous destination ports
- Large numbers of TCP SYN packets within a short period
- Repeated connection attempts against the same destination
- Multiple incomplete TCP connection attempts
- Rapid variation in destination ports

These behaviors could potentially be used by an IDS or SIEM detection
rule to identify network reconnaissance.

### Evidence Collected

- nmap-reconnaissance.pcapng - Original Wireshark packet capture
- wireshark-nmap-syn-scan.png - Screenshot of observed TCP SYN traffic

### Investigation Status

Status: In Progress

The reconnaissance activity has now been validated through packet-level
network analysis. The next phase of the investigation will focus on
detecting this behavior automatically using an IDS and forwarding
security events into the SIEM for investigation.