# Wireshark Lab: Network Traffic Analysis for Security Investigations

## Overview
This lab demonstrates my hands-on ability to use **Wireshark**, a leading network protocol analyzer, to inspect and filter packet capture (`.pcap`) data. As a security analyst, I analyzed real-world network traffic to identify source/destination IPs, examine protocols (TCP, UDP, DNS, ICMP), and extract payload details. This README documents my methodology and findings—skills critical for network monitoring, incident response, and threat hunting.

## Scenario (Fictional)
**Role**: Security Analyst  
**Task**: Investigate traffic to a website (`opensource.google.com`).  
**Goal**:  
- Identify source and destination IP addresses  
- Examine protocols used during the web browsing session  
- Analyze packet payloads and filtering strategies  

**Tools**: Wireshark (GUI), sample `.pcap` file (provided in lab environment)  
**Environment**: Windows VM with Wireshark pre-installed  

## Key Skills Demonstrated
- Opening and navigating packet capture files  
- Applying display filters (`ip.addr`, `ip.src`, `ip.dst`, `eth.addr`, `udp.port`, `tcp.port`, `tcp contains`)  
- Inspecting packet details (Frame, Ethernet II, IPv4, TCP, DNS layers)  
- Interpreting MAC addresses, IP addresses, TCP flags, ports, and payload text  
- Filtering DNS traffic (UDP port 53) and TCP HTTP traffic (port 80)  
- Searching for specific strings (`curl`) inside TCP payloads  

## Step-by-Step Lab Execution & Findings

### 1. Initial Exploration
- Opened `sample.pcap` – saw a multi‑protocol traffic mix (DNS, TCP, HTTP, ICMP).  
- **Observation**: First packet with "Echo (ping) request" used **ICMP** protocol.

### 2. Basic Filtering & Packet Deep Dive
- Applied `ip.addr == 142.250.1.139` → filtered to packets involving that IP.  
- Inspected a TCP packet’s details:  
  - **TCP destination port** = `80` (HTTP).  
  - Within IPv4 subtree: `Protocol` field indicated TCP.  
  - Ethernet II subtree showed MAC addresses.

### 3. Filtering by Source/Destination IP and MAC
- `ip.src == 142.250.1.139` → packets *from* that IP.  
- `ip.dst == 142.250.1.139` → packets *to* that IP.  
- `eth.addr == 42:01:ac:15:e0:02` → filtered by MAC address.  
  - For the first matching packet, the IPv4 `Protocol` field contained **TCP**.

### 4. DNS Traffic Analysis (UDP port 53)
- Filter `udp.port == 53` isolated DNS queries/responses.  
- Opened first packet → `Queries` section showed website name: `opensource.google.com`.  
- Opened fourth packet → `Answers` section returned the resolved IP address:  
  **`142.250.1.139`**.

### 5. TCP & HTTP Traffic Analysis (port 80)
- Filter `tcp.port == 80` → displayed all web traffic to `http://opensource.google.com`.  
- Inspected first packet (destination IP `169.254.169.254`):  
  - **Time to Live (IPv4)** = `64`  
  - **Frame Length** = `54 bytes`  
  - **Header Length (IPv4)** = `20 bytes`  
  - **Destination Address** = `169.254.169.254`  
- Applied `tcp contains "curl"` → found packets containing `curl` commands, proving payload‑level search capability.

## Tools & Commands Used (Wireshark Filters)
| Filter | Purpose |
|--------|---------|
| `ip.addr == 142.250.1.139` | Packets to/from that IP |
| `ip.src == ...` | Only source IP |
| `ip.dst == ...` | Only destination IP |
| `eth.addr == 42:01:ac:15:e0:02` | Filter by MAC address |
| `udp.port == 53` | DNS traffic |
| `tcp.port == 80` | HTTP traffic |
| `tcp contains "curl"` | Payload keyword search |

## Conclusion & Value to a Security Team
This lab proves I can:
- **Efficiently navigate** large packet captures using Wireshark’s GUI and coloring rules.  
- **Apply precise filters** to isolate malicious or suspicious traffic (IPs, MACs, protocols, ports, payloads).  
- **Drill into packet layers** (Ethernet, IP, TCP, DNS) to verify attributes like TTL, header length, flags, and queried domains.  
- **Identify DNS resolutions** and web traffic patterns – foundational for detecting C2 communications, data exfiltration, or policy violations.

With these skills, I can actively contribute to network security monitoring, incident analysis, and forensic investigations.

---

*Lab environment: Qwiklabs*  
*Date completed: 20 May 2026*  
*Hands‑on time: ~60 minutes*
