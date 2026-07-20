### Network Traffic Analysis & Incident Investigation

### Overview

This project demonstrates the investigation of suspicious internal network activity using Wireshark in a controlled home lab environment.

The objective was to capture and analyze real network traffic, identify reconnaissance techniques, reconstruct the sequence of events, and document the investigation in the same manner a Tier 1 Security Operations Center (SOC) analyst would perform an initial incident review.

Rather than simply identifying network protocols, this project focuses on understanding **why** specific traffic occurred, determining whether the observed behavior was expected or suspicious, and producing evidence-based conclusions supported by packet analysis.

---

## Objectives

- Capture live network traffic using Wireshark.
- Analyze common network protocols including ICMP, TCP, DNS, and SMB.
- Investigate simulated reconnaissance activity within a local network.
- Identify host discovery, port scanning, and SMB enumeration techniques.
- Reconstruct an incident timeline using captured packets.
- Produce a professional incident investigation report documenting findings and recommendations.

---

## Skills Demonstrated

- Network traffic capture and analysis
- Wireshark filtering and packet inspection
- TCP/IP protocol analysis
- ICMP connectivity analysis
- DNS request and response analysis
- TCP three-way handshake analysis
- TCP SYN scan investigation
- SMB protocol analysis
- Incident reconstruction
- Evidence-based reporting
- Security documentation
- Blue Team investigative methodology

---

## Tools Used

- Wireshark
- Nmap
- SMBClient
- Windows 11
- Ubuntu Linux
- Oracle VirtualBox
- PowerShell
- Windows Command Prompt

---

## Lab Environment

| System | Role | IP Address |
|---------|------|------------|
| Ubuntu Linux VM | Source Host | 10.0.2.15 |
| Windows 11 VM | Target Workstation | 10.0.2.3 |

The Windows workstation was configured with SMB enabled and Wireshark running to capture all inbound and outbound network traffic generated during the investigation.

---

## Investigation Scenario

The investigation simulates a common SOC alert involving unusual internal network activity.

The source workstation performs a series of actions against another internal host:

1. Verify connectivity using ICMP.
2. Perform a TCP SYN port scan.
3. Identify TCP port 445 (SMB) as accessible.
4. Attempt SMB share enumeration.
5. Review captured traffic to determine whether the observed behavior represents malicious activity or authorized administrative activity.

The investigation concludes with a written incident report documenting all findings.

---

## Investigation Workflow

```
Host Discovery (ICMP)
        │
        ▼
TCP SYN Port Scan
        │
        ▼
SMB Service Discovery
        │
        ▼
SMB Enumeration Attempt
        │
        ▼
Packet Analysis
        │
        ▼
Incident Timeline
        │
        ▼
Incident Report
```
## Network Diagram

       VirtualBox Lab

        Ubuntu VM
        10.0.2.15
             │
      ICMP / TCP / SMB
             │
             ▼
      Windows 11 VM
        10.0.2.3
      Wireshark Capture
---

## Network Traffic Analysis Investigation

### ICMP Host Discovery

The source host first verified that the destination workstation was online using ICMP Echo Requests and Echo Replies.

---

### TCP SYN Port Scan

An automated TCP SYN scan was observed probing approximately 1,000 common TCP ports to identify exposed services.

---

### SMB Enumeration

After discovering TCP port 445, the source host initiated SMB communication and attempted to enumerate available network shares.

---

### Traffic Analysis

Conversation statistics and packet analysis were used to reconstruct the complete sequence of events.

---

## Key Findings

The investigation identified the following sequence of activity:

- Successful ICMP host discovery
- Automated TCP SYN reconnaissance
- Discovery of TCP port 445
- SMB negotiation and share enumeration attempt
- No successful authentication
- No evidence of exploitation
- No evidence of persistence or data exfiltration

---

## Lessons Learned

This project reinforced several core blue team concepts:

- Individual packets rarely provide enough context to determine malicious intent.
- Reconnaissance activity may closely resemble legitimate administrative behavior.
- Packet captures should always be correlated with system context before drawing conclusions.
- Effective incident response depends on careful observation, evidence collection, and structured documentation rather than assumptions.

---

## Files Included

| File | Description |
|------|-------------|
| README.md | Project overview and documentation |
| Incident Response - Network Traffic Analysis.md | Complete investigation report |
| Network_Traffic_Analysis.pcapng | Packet capture used during analysis |
| screenshots/ | Supporting Wireshark and Linux CLI screenshots |

---

## Future Improvements

Future versions of this project may include:

- Sysmon event correlation
- Wazuh SIEM integration
- Sigma detection rules
- Zeek network monitoring
- Automated detection of reconnaissance activity
- Correlation between network traffic and Windows Event Logs

---

## About This Project

This project is the first in a series of hands-on cybersecurity labs documenting my progression toward a career in Blue Team operations and cloud security.
