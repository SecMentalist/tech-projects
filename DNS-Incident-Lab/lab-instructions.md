# Lab Instructions – DNS Incident Analysis

## Scenario
You are a cybersecurity analyst. Several users report that `www.space.com` is inaccessible. When they try to load the page, they see the error: **“destination port unreachable”**.

Your task is to investigate using a network protocol analyser (`tcpdump`) and determine the root cause.

## Step 1 – Understand the expected behaviour
Normally, when you visit a website:
1. Your browser sends a **DNS query** (UDP, port 53) to a DNS server to resolve the domain name to an IP address.
2. The DNS server replies with the IP address.
3. Your browser then sends an HTTPS request to that IP address.

If DNS fails, the website cannot be reached.

## Step 2 – Review the captured data
Open the file `assets/tcpdump.log` in any text editor. This log was captured while you attempted to visit `www.space.com`.

### Example line from the log:

13:24:32.192571 IP 192.51.100.15.54321 > 203.0.113.2.53: 35084+ A? www.space.com. (42)
text


**Key fields to recognise:**
- `13:24:32.192571` – timestamp (1:24 PM, 32.192571 seconds)
- `192.51.100.15` – source IP (your computer)
- `203.0.113.2` – destination IP (DNS server)
- `.53` – destination port (DNS service)
- `A? www.space.com` – DNS query for an A record (IPv4 address)

### The response line:

13:24:32.192573 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2 udp port 53 unreachable, length 36
text


This is an **ICMP Destination Unreachable** error, specifically “udp port 53 unreachable”.

## Step 3 – Answer the analysis questions
Use the log to fill out the `student-worksheet.md`. The questions are:
1. What timestamp did the incident first occur?
2. Which source IP sent the DNS query? Which destination IP?
3. What port number was the DNS query sent to? What service normally uses that port?
4. What protocol and error message were returned?
5. How many times did the query fail (look for repeated attempts)?

## Step 4 – Complete the incident report
Using the template in `student-worksheet.md`, write a one‑page incident report that includes:
- **Summary** of the problem
- **How the IT team became aware** (user reports)
- **Investigation steps** (what you did)
- **Key findings** (ports, IPs, protocols, error message)
- **Likely cause** (what does “udp port 53 unreachable” mean for the DNS server?)
- **Recommended next steps** (e.g., restart DNS service, check firewall)

## Step 5 – Self‑check (optional)
Compare your answers with the expected key points listed at the end of `student-worksheet.md` (if provided by your instructor). Otherwise, review the logic:  
*“No service is listening on port 53 – the DNS daemon is down or blocked.”*

## Estimated time to complete: 30–45 minutes
