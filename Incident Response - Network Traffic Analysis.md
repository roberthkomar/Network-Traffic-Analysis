### Network Traffic Analysis
### Internal Reconnaissance Investigation

---

# Executive Summary

An alert was generated after unusual internal network activity was detected between two hosts within the lab environment.

The objective of this investigation was to determine the sequence of events, identify the protocols involved, assess whether the observed behavior was malicious or legitimate, and document all findings using packet analysis in Wireshark.

The investigation found that one internal host first verified connectivity using ICMP, then performed a TCP SYN port scan against the Windows workstation. After discovering TCP port 445 (SMB) was open, the host attempted to enumerate available SMB resources. No exploitation, malware execution, privilege escalation, persistence, or data exfiltration was observed. Based on the known context of the lab environment, the activity was determined to be authorized security testing.

---

# Lab Environment

| Host       | IP Address | Operating System |     Role    |
|------------|------------|------------------|-------------|
|  Ubuntu VM |  10.0.2.15 |   Ubuntu Linux   | Source Host |
| Windows VM |   10.0.2.3 |     Windows 11   | Target Host |

---

# Investigation Timeline

### Event 1 – Host Discovery

The investigation began with ICMP Echo Requests sent from the Ubuntu workstation to the Windows workstation.

Packet analysis showed that all Echo Requests received successful Echo Replies with no packet loss, confirming that the target host was online and reachable.

This type of traffic is commonly generated during routine network troubleshooting, monitoring, and administrative tasks. However, it is also frequently observed during the reconnaissance phase of an intrusion, where an attacker first verifies that a target system is active before performing additional actions.

---

### Event 2 – Port Reconnaissance

Shortly after verifying connectivity, the source host initiated a TCP SYN scan against the Windows workstation using Nmap.

The packet capture showed SYN packets being sent rapidly to approximately 1,000 common TCP ports. The scan identified TCP port 445 as accessible while the remaining ports were either closed or filtered according to the system's firewall configuration.

The high volume of sequential connection attempts indicated automated scanning behavior rather than normal user activity.

Port scanning is a common reconnaissance technique used by system administrators, vulnerability scanners, penetration testers, and threat actors. Although the behavior appears suspicious, the network traffic alone is insufficient to determine malicious intent without additional context.

---

### Event 3 – SMB Enumeration

After identifying TCP port 445 as open, the source host initiated communication using the Server Message Block (SMB) protocol.

The packet capture showed successful TCP session establishment followed by SMB negotiation and an attempt to enumerate available network shares.

The Windows workstation denied anonymous access, preventing the enumeration from succeeding.

Enumeration of SMB services is commonly performed during security assessments and administrative tasks but is also a well-known reconnaissance technique used by attackers seeking information about available network resources.

No evidence of successful authentication or unauthorized access was observed during this investigation.

---

# Protocol Analysis

## ICMP

ICMP traffic was used to verify connectivity between the two hosts.

Successful Echo Replies confirmed that the destination system was online and responding normally.

Although ICMP is commonly associated with network troubleshooting, analysts should recognize that it is also frequently used during the initial stages of reconnaissance.

---

## TCP

TCP was used during both the Nmap scan and the SMB communication.

Analysis of the TCP three-way handshake confirmed successful connection establishment before higher-layer protocols began exchanging data.

Reviewing TCP flags and connection attempts allowed the sequence of events to be reconstructed accurately.

---

## SMB

SMB traffic was observed over TCP port 445.

Packet analysis showed session negotiation followed by an attempt to enumerate available resources.

The server rejected anonymous enumeration, indicating that access controls were functioning as expected.

---

# Findings

The investigation determined the following sequence of events:

- The source host verified that the target system was online using ICMP.
- The source host performed an automated TCP SYN scan against the target workstation.
- TCP port 445 was identified as accessible.
- The source host initiated SMB communication and attempted anonymous share enumeration.
- The enumeration attempt was unsuccessful because anonymous access was not permitted.
- No evidence of exploitation or unauthorized access was identified.

---

# Risk Assessment

The observed behavior closely resembles the reconnaissance phase of the Cyber Kill Chain.

Reconnaissance activities are not inherently malicious. Similar traffic may be generated by:

- Authorized penetration testing
- Vulnerability scanners
- Network inventory tools
- System administrators
- Security researchers

Because no exploitation or follow-on malicious activity was observed, the packet capture alone does not indicate that a compromise occurred.

Analysts should always correlate network evidence with asset ownership, user activity, maintenance schedules, and other available telemetry before determining intent.

---

# Recommendations

If this activity were observed in a production environment, I would recommend the following actions:

- Verify whether the source host belongs to an authorized administrator or vulnerability scanner.
- Confirm whether the scanning activity was scheduled or approved.
- Review authentication logs for unsuccessful SMB logon attempts.
- Continue monitoring the source host for additional reconnaissance or lateral movement.
- Ensure SMB access is restricted to authorized users and systems.
- Maintain detailed logging for future investigations.

---

# Conclusion

Analysis of the packet capture identified behavior consistent with internal reconnaissance.

The source host successfully confirmed host availability, performed an automated TCP SYN scan, identified TCP port 445 as available, and attempted to enumerate SMB resources.

Although these actions are commonly associated with attacker reconnaissance, they are also routinely performed during legitimate administrative and security assessment activities.

This investigation demonstrates the importance of analyzing network evidence within its operational context rather than relying solely on individual indicators when determining whether activity is malicious.