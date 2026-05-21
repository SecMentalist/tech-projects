# Suricata Custom Rules & Alerts Lab

## Overview

In this hands‑on lab, I explored how to use **Suricata** – an open‑source IDS/IPS and network analysis tool – to monitor network traffic, trigger alerts based on custom rules, and analyse log outputs.  
I learned how to:

- Examine the composition of Suricata rules (action, header, rule options).
- Trigger custom rules using a sample packet capture (`.pcap`) file.
- Interpret alert logs in `fast.log` and detailed JSON events in `eve.json`.

The lab simulates a real‑world scenario where a security analyst monitors traffic on a corporate network.

---

## Scenario (Fictional)

I work as a junior security analyst at a mid‑sized company. My task is to monitor network traffic for suspicious HTTP requests. The company uses Suricata as its IDS. I have been provided with:

- `sample.pcap` – a packet capture file containing example network traffic.
- `custom.rules` – a rule file that I will edit and test.

I need to verify that Suricata correctly alerts on HTTP GET requests leaving the internal network, and then analyse the logs generated.

---

## Environment Setup

| Item                          | Details                                      |
|-------------------------------|----------------------------------------------|
| **User**                      | `analyst` (already logged into a Bash shell) |
| **Working directory**         | `/home/analyst`                              |
| **Key files**                 | `custom.rules`, `sample.pcap`                |
| **Log directory**             | `/var/log/suricata/` (contains `fast.log`, `eve.json`) |
| **Suricata variable**         | `$HOME_NET` = `172.21.224.0/20`              |

> **Note:** Using `sudo` is required in this lab to process the `.pcap` file with Suricata.

---

## Tasks Performed

### Task 1 – Examine a Custom Rule in Suricata

I started by looking at the existing rule inside `custom.rules`:

```bash
cat custom.rules

Output:
text

alert http $HOME_NET any -> $EXTERNAL_NET any (msg:"GET on wire"; flow:established,to_server; content:"GET"; http_method; sid:12345; rev:3;)

Understanding the rule components:
Component	Value / Meaning
Action	alert – generate an alert when traffic matches.
Protocol	http – rule applies only to HTTP traffic.
Header	$HOME_NET any -> $EXTERNAL_NET any – from any port on the home network to any port on an external network.
Rule options	msg:"GET on wire" – alert text.
flow:established,to_server – match established client→server packets.
content:"GET" – look for the string "GET".
http_method – restrict the content match to the HTTP method.
sid:12345; rev:3 – unique signature ID and revision number.

This rule triggers an alert every time Suricata sees an HTTP GET request from our internal network to an external server.
Task 2 – Trigger a Custom Rule in Suricata

Before running Suricata, the log directory was empty:
bash

ls -l /var/log/suricata
# total 0

I executed Suricata with the custom rule and the sample packet capture:
bash

sudo suricata -r sample.pcap -S custom.rules -k none

Explanation of options:

    -r sample.pcap – read traffic from the .pcap file.

    -S custom.rules – use rules from custom.rules.

    -k none – disable checksum validation (since we are using a static capture file).

After running the command, Suricata processed the packets and created log files:
bash

ls -l /var/log/suricata
# total 16
# -rw-r--r-- 1 root root   xxx fast.log
# -rw-r--r-- 1 root root   xxx eve.json
# ...

I examined the fast.log (a legacy, human‑readable alert log):
bash

cat /var/log/suricata/fast.log

Output:
text

11/23/2022-12:38:34.624866  [**] [1:12345:3] GET on wire [**] [Classification: (null)] [Priority: 3] {TCP} 172.21.224.2:49652 -> 142.250.1.139:80
11/23/2022-12:38:58.958203  [**] [1:12345:3] GET on wire [**] [Classification: (null)] [Priority: 3] {TCP} 172.21.224.2:58494 -> 142.250.1.139:80

Each line confirms that the rule was triggered twice – two HTTP GET requests from the internal host 172.21.224.2 to external IP 142.250.1.139 (port 80).
Task 3 – Examine eve.json Output

The eve.json file contains rich, structured JSON data for every event. I first viewed the raw content:
bash

cat /var/log/suricata/eve.json

The output was dense, so I used jq to format it nicely and page through it:
bash

jq . /var/log/suricata/eve.json | less

(Use f/b to scroll, q to exit.)

Key findings from jq queries:

    Severity of the first alert:
    
    Value: 3 (default severity for this rule)

    Destination IP of the last event:
    After extracting .dest_ip for all events, the last entry showed 142.250.1.102.

    Alert signature for the first alert entry:
    "GET on wire" (matching the msg from our rule).

To see all logs belonging to the same network flow (same flow_id), I used:
bash

jq 'select(.flow_id==1647223379236084)' /var/log/suricata/eve.json

This returned all packets associated with that flow, demonstrating how flow_id helps correlate related traffic.
Conclusion

Through this lab, I gained practical experience with Suricata:

    Rule creation & syntax – I can now write and interpret custom rules (action, header, options like msg, content, flow).

    Triggering alerts – I successfully ran Suricata against a .pcap file and verified that alerts appear in fast.log.

    Log analysis – I learned that eve.json provides far more detail than fast.log and is the preferred source for incident response. Using jq, I can filter and extract specific fields (timestamp, flow_id, signature, protocol, IPs) or correlate events by flow_id.

This hands‑on exercise is a key step toward becoming a proficient security analyst who can monitor network traffic, write detection rules, and investigate alerts.
