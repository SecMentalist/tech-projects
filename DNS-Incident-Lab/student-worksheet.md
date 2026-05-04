# DNS Incident Analysis – My Personal Lab Worksheet

**Author:** Robert Ursache (SecMentalist)  
**Purpose:** I created this worksheet to practise analysing DNS failures with `tcpdump`. I will fill it out as I work through the lab. This demonstrates my ability to investigate network incidents and write professional reports.

---

## Scenario (Fictional – I designed this for my own practice)

I am a cybersecurity analyst. Several customers of a client report that they cannot access `www.space.com`. They see the error **“destination port unreachable”** after waiting for the page to load.

I am tasked with analysing the situation and determining which network protocol was affected. When I try to visit the website myself, I also receive the same error.

To troubleshoot, I load `tcpdump` and attempt to load the webpage again. Normally, the browser sends a DNS query (UDP, port 53) to a DNS server to resolve the domain name. Then it uses the returned IP address to send an HTTPS request. However, the analyser shows that when I send UDP packets to the DNS server, I receive ICMP packets containing the error: **“udp port 53 unreachable.”**

### The `tcpdump` log (from my lab)

I have saved a sample log in `assets/tcpdump.log`. Here is what it contains:

1. First two lines: outgoing UDP request from my computer to the DNS server asking for the IP of `space.com`.
2. Third and fourth lines: the response – an ICMP error from `203.0.113.2` indicating the UDP packet could not be delivered to port 53.
3. Timestamp of the incident: `13:24:32.192571` (1:24 p.m., 32.192571 seconds).
4. Source IP (my computer): `192.51.100.15` → Destination IP (DNS server): `203.0.113.2.domain`.
5. Query ID `35084+` and flag `A?` (request for an A record).
6. Error message: `udp port 53 unreachable` – meaning no service was listening on port 53 of the DNS server.
7. The same error occurred two more times (three attempts total).

---

## My Completed Incident Report

I will now write my incident report based on my analysis.

---

### Cybersecurity Incident Report – Network Traffic Analysis

**Report author:** Robert Ursache  
**Date:** 4 May 2026

#### Part 1: Summary of the Problem (DNS & ICMP Traffic Log)

Based on my `tcpdump` analysis, the issue is a **failure of DNS resolution** due to an unreachable DNS service.

- The browser sent a UDP request to the DNS server on destination port **53** to resolve `www.space.com`.
- Instead of a valid DNS response, the server replied with an **ICMP Destination Unreachable** error message: **“udp port 53 unreachable”**.
- This means **no service was listening on port 53** of the DNS server – the DNS daemon was either down, misconfigured, or blocked.
- The error occurred repeatedly (three attempts). As a result, the browser never received the website’s IP address, causing the `“destination port unreachable”` error for users.

**Impact:** The website became inaccessible because domain name resolution failed. The affected protocol is **DNS (UDP port 53)**, and the **ICMP** protocol carried the error notification.

**What the UDP protocol reveals:** The DNS server was not listening on port 53, because the ICMP error message “udp port 53 unreachable” was returned instead of a valid DNS response.

**Port noted in the error message:** Port **53** – used for **DNS** service.

**Most likely issue:** The DNS service (daemon) on the DNS server is down, not running, or not listening on port 53.

---

#### Part 2: Investigation Analysis & Likely Cause

**Incident Time**  
`13:24:32.192571` (1:24 p.m., 32.192571 seconds)

**How I Became Aware**  
Several customers of the client reported that they could not access `www.space.com` and saw the error “destination port unreachable”. These user reports triggered my investigation.

**Actions I Took to Investigate**  
1. **Reproduced the issue** – I visited the website and confirmed the same error.  
2. **Loaded `tcpdump`** – I used the network protocol analyser to capture live traffic while loading the webpage again.  
3. **Analysed the captured packets** – I examined the `tcpdump` log and identified:  
   - Outgoing UDP request to the DNS server (port 53) for `space.com`.  
   - Incoming ICMP error response containing “udp port 53 unreachable”.  
   - Timestamps, source/destination IP addresses, and repeated failures.  
4. **Identified the root cause** – The DNS server was not listening on port 53, meaning the DNS service was down or unreachable.  
5. **Reported the findings** – I escalated the issue to my direct supervisor and security engineers.

**Key Findings**  
- **Affected port** – UDP port 53 (standard for DNS)  
- **DNS server IP** – `203.0.113.2`  
- **Error message** – ICMP Destination Unreachable: “udp port 53 unreachable”  
- **Protocols involved** – UDP (DNS request) and ICMP (error)  
- **Root cause** – No service listening on port 53; DNS daemon down, misconfigured, or blocked  
- **Result** – Browser could not resolve domain name → “destination port unreachable” error  
- **Repeat attempts** – Three failures, confirming persistence  

**Likely Cause of the Incident**  
The DNS service (daemon) on the DNS server (`203.0.113.2`) was either **stopped, crashed, or not properly running**. Because no process was bound to UDP port 53, the server’s network stack responded with an ICMP “port unreachable” error instead of a DNS answer. This could result from an accidental service stop, a system misconfiguration, a crash, or a firewall rule that blocked only the DNS port while still allowing ICMP responses.

**Recommended Next Steps**  

Based on the likely cause (DNS service stopped, crashed, or blocked), here are the recommended actions to resolve the incident and prevent recurrence:

1. **Verify DNS service status** – Log into the DNS server (`203.0.113.2`) and check if the DNS daemon (e.g., `named`, `systemd-resolved`, or Windows DNS Server) is running. If stopped, start it immediately.

2. **Check service logs** – Examine DNS server logs for errors or crash indicators (e.g., `/var/log/messages`, `/var/log/syslog`, or Event Viewer). Look for configuration issues, resource exhaustion, or unauthorised stop commands.

3. **Review firewall rules** – Ensure that UDP port 53 is allowed on the DNS server’s local firewall and any network firewalls. The ICMP response indicates the server is reachable, but a firewall may be blocking only port 53.

4. **Restart DNS service** – If the service is running but unresponsive, restart it gracefully. Test resolution with `nslookup space.com 203.0.113.2` after restart.

5. **Set up monitoring** – Configure alerts for DNS service health (e.g., using Nagios, Zabbix, or a simple cron job) to detect future stops or crashes.

6. **Document the incident** – Record the root cause, resolution steps, and any preventive measures in the knowledge base for future reference.

> **Note:** All the following actions must be performed on the DNS server itself (`203.0.113.2`), not on the analyst’s workstation where `tcpdump` was run.

---

## My Reflection (Optional)

After completing this lab, I can confidently:

- Read `tcpdump` logs and identify DNS queries and ICMP errors.
- Recognise that “udp port 53 unreachable” means the DNS service is not listening.
- Write a clear incident report that summarises the problem, investigation steps, key findings, and likely cause.

This lab is part of my cybersecurity portfolio. It shows my practical skills in network traffic analysis and incident documentation.

**Created by Robert Ursache** – [GitHub: SecMentalist](https://github.com/SecMentalist)
