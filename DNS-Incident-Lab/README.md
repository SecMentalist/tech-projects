# DNS Incident Analysis Lab

## Objective
Learn how to analyse a real‑world‑style DNS outage using `tcpdump` logs. Practice identifying the affected protocol, interpreting ICMP error messages, and writing a professional cybersecurity incident report.

## Scenario (Fictional)
Users report that `www.space.com` is inaccessible and shows the error **“destination port unreachable”**. As a cybersecurity analyst, you use `tcpdump` to capture network traffic. The log reveals an ICMP error: **“udp port 53 unreachable”** – indicating that the DNS service is not responding.

## Lab Files
| File | Description |
|------|-------------|
| `lab-instructions.md` | Step‑by‑step tasks to complete the lab |
| `student-worksheet.md` | Template for answers and the final incident report |
| `assets/tcpdump.log` | Sample packet capture (fictional) |

## What You Will Learn
- Read and interpret `tcpdump` output (timestamps, IP addresses, ports, error messages)
- Recognise the role of DNS (UDP port 53) and ICMP Destination Unreachable
- Correlate user reports with packet analysis
- Document findings in a structured incident report

## How to Use This Lab
1. **Clone or download** this repository.
2. Read `lab-instructions.md` to understand the tasks.
3. Open `assets/tcpdump.log` in any text editor.
4. Complete the questions in `student-worksheet.md`.
5. Write the incident report using the template provided.

## Estimated Time
30‑45 minutes

## Prerequisites
- Basic understanding of networking (DNS, UDP, ICMP)
- No live internet required – all data is fictional

## Author
Robert Ursache – Cybersecurity portfolio project

## License
MIT – feel free to use and adapt for learning or teaching.
