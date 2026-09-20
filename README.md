# Web Attack Detection & Investigation

## Overview

This project demonstrates a SOC-style investigation of suspicious network activity within a controlled cybersecurity lab.

I generated network activity from a Kali Linux machine against a Metasploitable target and captured the traffic using Wireshark. I then analyzed TCP connection patterns to identify characteristics associated with network reconnaissance and port-scanning activity.

The investigation focused on identifying the source and destination systems, analyzing TCP flags, distinguishing open and closed port responses, and documenting the activity from a defensive security perspective.

## Lab Environment

- Kali Linux — Investigation / traffic source
- Metasploitable 2 — Target system
- Oracle VirtualBox — Virtualized lab environment
- Wireshark — Packet capture and traffic analysis
- Nmap — Network reconnaissance and port scanning
- Host-only network — Isolated lab communication

## Investigation

### 1. Network Validation

The lab network was validated before beginning the investigation.

Kali Linux was configured with the host-only IP address:

`192.168.56.20`

The Metasploitable target was identified as:

`192.168.56.103`

Connectivity between the systems was confirmed using ICMP ping requests with 0% packet loss.

### 2. Traffic Capture

Wireshark was configured to capture traffic on the `eth1` host-only interface.

Initial HTTP communication between Kali Linux and Metasploitable was observed, confirming that traffic between the two systems could be captured successfully.

### 3. Port Scan Activity

A TCP SYN scan was performed against the Metasploitable system while Wireshark captured the resulting network traffic.

The scan identified multiple exposed services including FTP, SSH, Telnet, SMTP, DNS, HTTP, SMB and other services.

### 4. TCP SYN Analysis

The following Wireshark display filter was used to isolate SYN packets originating from the Kali Linux system:

`ip.src == 192.168.56.20 && tcp.flags.syn == 1 && tcp.flags.ack == 0`

A large number of SYN packets were observed from the same source system to numerous destination ports on the target within a short period.

This pattern is consistent with TCP port-scanning/reconnaissance activity.

### 5. Open-Port Response Analysis

SYN/ACK responses from the target were examined to identify ports accepting TCP connections.

The traffic demonstrated the expected TCP behavior:

`SYN → SYN/ACK`

Multiple services responded with SYN/ACK packets, correlating with ports identified as open during the Nmap scan.

### 6. Closed-Port Response Analysis

RST/ACK responses from the target were also examined.

Numerous reset responses were observed from the target following connection attempts to closed ports.

This demonstrated the contrasting network behavior between open and closed TCP ports.

## Key Findings

- A single source generated connection attempts across numerous destination ports.
- SYN packets occurred rapidly within a short period.
- Open ports responded with SYN/ACK packets.
- Closed ports responded with TCP reset packets.
- Wireshark traffic correlated with the reconnaissance activity observed through Nmap.
- The traffic pattern provides network-level indicators that could be used to investigate potential scanning activity in a SOC environment.

## Skills Demonstrated

- Network traffic analysis
- Wireshark filtering
- TCP/IP analysis
- Port-scan detection
- Network reconnaissance analysis
- Nmap
- Incident investigation
- SOC investigation methodology
- Evidence collection and documentation

## Security Note

All activity documented in this project was performed in an isolated, controlled virtual lab using systems specifically configured for cybersecurity training.
