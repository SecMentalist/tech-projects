# Network Traffic Analysis with tcpdump

## Overview

In this lab, I acted as a security analyst tasked with capturing and analyzing live network traffic on a Linux virtual machine using `tcpdump`. The goal was to identify network interfaces, inspect live traffic, save captured packets to a file, and apply filters to examine specific data.

This hands-on activity demonstrates fundamental skills for network monitoring, packet analysis, and incident investigation in a Linux environment.

## Scenario

As a network analyst, I needed to use `tcpdump` to capture and analyze network traffic from a Linux terminal. The lab environment provided a pre‑logged‑in user account named `analyst` with a home directory containing a sample packet capture file for later analysis. All tasks were performed using command‑line tools.

## Lab Environment

- **OS**: Linux (Ubuntu‑based virtual machine)  
- **User**: `analyst`  
- **Key Tools**: `ifconfig`, `tcpdump`, `curl`, `ls`  
- **Network Interface**: `eth0` (Ethernet interface)

---

## Tasks Performed

### 1. Identify Network Interfaces

Before capturing traffic, I listed all available network interfaces to determine the correct one for monitoring.

**Commands used:**

```bash
sudo ifconfig
sudo tcpdump -D

Output example (ifconfig):
text

eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1460
        inet 172.17.0.2  netmask 255.255.0.0  broadcast 172.17.255.255
        ...
lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        ...

Findings:
The Ethernet interface eth0 was active and suitable for capturing live network traffic. The loopback interface lo was also present but not used for external traffic analysis.
2. Inspect Live Network Traffic with tcpdump

I captured a small sample of live packets from eth0 to understand the structure of captured data.

Command:
bash

sudo tcpdump -i eth0 -v -c5

Options explained:

    -i eth0 – capture on the eth0 interface

    -v – verbose output (detailed packet information)

    -c5 – capture only 5 packets, then exit

Sample output (partial):
text

tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
10:57:33.427749 IP (tos 0x0, ttl 64, id 35057, offset 0, flags [DF], proto TCP (6), length 134)
  7acb26dc1f44.5000 > nginx-us-east1-c...59788: Flags [P.], cksum 0x5851, seq 1080713945:1080714027, ack 62760789, win 501, length 82
...

Key observations:

    Timestamp, protocol (IP), and header flags (e.g., [P.] for push + ACK).

    Source and destination addresses with port numbers.

    Sequence/acknowledgment numbers and payload length.

3. Capture Network Traffic to a File

To save traffic for later analysis, I captured packets on port 80 (HTTP) and wrote them to a .pcap file.

Command (run in background):
bash

sudo tcpdump -i eth0 -nn -c9 port 80 -w capture.pcap &

Options explained:

    -nn – disable name resolution (IP and port) – security best practice

    -c9 – capture 9 packets, then stop

    port 80 – filter only HTTP traffic

    -w capture.pcap – write raw packets to the specified file

    & – run in background, returning the prompt

Generate HTTP traffic:
bash

curl opensource.google.com

Verify capture:
bash

ls -l capture.pcap

Output showed the file was created successfully.
4. Filter and Analyze the Captured Packet Data

I used tcpdump to read the saved capture.pcap file and applied different output formats.

a) Verbose packet headers:
bash

sudo tcpdump -nn -r capture.pcap -v

b) Hexadecimal and ASCII dump (for deep analysis):
bash

sudo tcpdump -nn -r capture.pcap -X

Sample output (-v):
text

reading from file capture.pcap, link-type EN10MB (Ethernet)
20:53:27.669101 IP (tos 0x0, ttl 64, id 50874, offset 0, flags [DF], proto TCP (6), length 60)
    172.17.0.2:46498 > 146.75.38.132:80: Flags [S], seq 4197622953, win 65320, length 0
...

The -X option displayed both hexadecimal representation and ASCII characters, useful for spotting anomalies or embedded data.
What I Learned

Through this lab, I gained practical experience with:

    Interface discovery – using ifconfig and tcpdump -D to identify active network interfaces.

    Live traffic capture – applying filters (count, port, verbosity) to focus on relevant packets.

    Saving captures – writing raw packet data to a .pcap file for offline analysis.

    Reading .pcap files – using tcpdump -r with options like -v (verbose) and -X (hex/ASCII) to inspect packet details.

    Security best practices – disabling name resolution (-nn) to avoid leaking investigative activity and to ensure data integrity.

    Interpreting packet fields – recognizing TCP flags, sequence numbers, ports, and payload lengths.

This lab simulated a real‑world scenario where a security analyst must quickly capture and filter network traffic to investigate anomalies or gather evidence. The skills I practiced are directly applicable to incident response, network forensics, and threat hunting.
Tools & Commands Reference
Command	Purpose
sudo ifconfig	List network interfaces with details
sudo tcpdump -D	List interfaces available for capture
sudo tcpdump -i eth0 -v -c5	Capture 5 live packets from eth0 (verbose)
sudo tcpdump -i eth0 -nn -c9 port 80 -w capture.pcap &	Capture 9 HTTP packets to a file (no name resolution)
curl <url>	Generate HTTP traffic for capture
sudo tcpdump -nn -r capture.pcap -v	Read and display verbose headers from a .pcap file
sudo tcpdump -nn -r capture.pcap -X	Display hex+ASCII dump from a .pcap file
Conclusion

This lab provided a controlled environment to master essential tcpdump workflows. I can now confidently capture live traffic, filter by ports or protocols, save packet data, and perform forensic analysis on .pcap files. These skills are foundational for any security analyst working with network data in Linux.
