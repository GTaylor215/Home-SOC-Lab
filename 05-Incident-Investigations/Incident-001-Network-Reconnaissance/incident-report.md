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



---

## IDS Detection - Suricata

After manually identifying the reconnaissance behavior through Wireshark,
Suricata was deployed as a network Intrusion Detection System (IDS) to
automatically detect similar activity.

### IDS Configuration

Suricata was configured to monitor the Kali Linux eth0 interface.

HOME_NET:

192.168.56.0/24

The Emerging Threats ruleset was installed using suricata-update.

Initial testing showed that the default rules did not generate an alert
for the controlled Nmap scan. A custom detection rule was therefore
created to detect repeated TCP SYN activity between the Kali attacker
and Windows victim.

### Custom Detection Rule

Rule SID: 1000001

Alert Message:

SOC LAB - Possible TCP Port Scan

Detection Logic:

- Protocol: TCP
- Source: 192.168.56.102 (Kali Linux)
- Destination: 192.168.56.101 (Windows)
- TCP SYN flag monitored
- Alert threshold: 20 matching packets within 10 seconds
- Tracking method: Source IP

### Detection Validation

The Suricata configuration and custom rule were validated before
starting the IDS.

Suricata was then configured to monitor eth0 while another controlled
Nmap scan was launched against the Windows endpoint.

The scan successfully triggered the custom detection rule and generated
the following alert:

SOC LAB - Possible TCP Port Scan

This confirmed that Suricata could automatically identify the
reconnaissance behavior that had previously been identified manually
through Wireshark.

### Detection Analysis

The investigation demonstrated the difference between network
visibility and automated threat detection.

Wireshark provided visibility into the individual TCP SYN packets and
allowed the scanning behavior to be identified manually.

Suricata applied detection logic to the network traffic and
automatically generated an alert once the configured threshold was met.

This created the following detection workflow:

Nmap reconnaissance
        |
        v
Network traffic
        |
        v
Suricata IDS
        |
        v
Custom detection rule
        |
        v
SOC LAB - Possible TCP Port Scan alert

### Evidence Collected

- local.rules - Custom Suricata TCP port scan detection rule
- fast.log - Suricata alert log containing the triggered detection
- nmap-reconnaissance.pcapng - Original network packet capture
- suricata-port-scan-alert.png - Screenshot of triggered IDS alert
- suricata-custom-port-scan-rule.png - Screenshot of custom detection rule
- suricata-rule-validation.png - Screenshot of successful rule validation
- suricata-detection-nmap-scan.png - Nmap activity used to trigger the detection

### Investigation Status

Status: In Progress

Network reconnaissance has now been manually identified using Wireshark
and automatically detected using Suricata IDS.

The next phase of the investigation will focus on centralizing security
telemetry and alerts within a SIEM for SOC analyst monitoring and
investigation.