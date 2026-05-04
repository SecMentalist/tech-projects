# Lab Instructions – DNS Incident Analysis (My Personal Lab)

## Scenario
I am a cybersecurity analyst. Several users report that `www.space.com` is inaccessible. When they try to load the page, they see the error: **“destination port unreachable”**.

My task is to investigate using a network protocol analyser (`tcpdump`) and determine the root cause.

## Step 1 – Understand the expected behaviour
Normally, when I visit a website:
1. My browser sends a **DNS query** (UDP, port 53) to a DNS server to resolve the domain name to an IP address.
2. The DNS server replies with the IP address.
3. My browser then sends an HTTPS request to that IP address.

If DNS fails, the website cannot be reached.

## Step 2 – Review the captured data
I open the file `assets/tcpdump.log` in any text editor. This log was captured while I attempted to visit `www.space.com`.
### The actual log content:

13:24:32.192571 IP 192.51.100.15.52484 > 203.0.113.2.domain: 35084+ A? space.com. (24)

13:24:36.098564 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2 udp port 53 unreachable length 254

13:26:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A? space.com. (24)

13:27:15.934126 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2 udp port 53 unreachable length 320

13:28:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A? space.com. (24)

13:28:50.022967 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2 udp port 53 unreachable length 150

### Key fields I need to recognise (using the first query as an example):

- `13:24:32.192571` – timestamp (1:24 PM, 32.192571 seconds)
- `192.51.100.15` – source IP (my computer)
- `52484` – source port (random high port)
- `203.0.113.2` – destination IP (DNS server)
- `.domain` – destination port 53 (DNS service)
- `A? space.com` – DNS query for an A record (IPv4 address)

### The response (error message):

`13:24:36.098564 IP 203.0.113.2 > 192.51.100.15: ICMP 203.0.113.2 udp port 53 unreachable length 254`

This is an **ICMP Destination Unreachable** error, specifically **“udp port 53 unreachable”**. The `length` varies (254, 320, 150) – that is normal for different packet sizes.

## Step 3 – Answer the analysis questions
I use the log to fill out `student-worksheet.md`. The questions are:
1. What timestamp did the incident first occur?
2. Which source IP sent the DNS query? Which destination IP?
3. What port number was the DNS query sent to? What service normally uses that port?
4. What protocol and error message were returned?
5. How many times did the query fail (look for repeated attempts)?

## Step 4 – Complete the incident report
Using the template in `student-worksheet.md`, I write a one‑page incident report that includes:
- **Summary** of the problem
- **How the IT team became aware** (user reports)
- **Investigation steps** (what I did)
- **Key findings** (ports, IPs, protocols, error message)
- **Likely cause** (what does “udp port 53 unreachable” mean for the DNS server?)
- **Recommended next steps** (e.g., restart DNS service, check firewall)

## Estimated time to complete: 30–45 minutes
