# Windows DNS Server Configuration in VirtualBox

## Description

This project demonstrates the installation and configuration of a DNS server on Windows Server 2022 within a virtual lab environment. A Windows 11 client is configured to use the DNS server for name resolution. The setup proves both forward and reverse DNS lookups work correctly.

## Tools & Environment

- **VirtualBox** (virtualization platform)
- **Windows Server 2022** (DNS server)
- **Windows 11** (DNS client)
- **Internal Network** (isolated VirtualBox network)

## Network Configuration

| Machine | IP Address | DNS Server |
|---------|-----------|-------------|
| Windows Server 2022 | 192.168.10.1 | 127.0.0.1 (self) |
| Windows 11 | 192.168.10.2 | 192.168.10.1 |

- **Subnet mask:** 255.255.255.0
- **Domain name:** `Area51.Local`

## Setup & Configuration Steps

### 1. Create Virtual Machines in VirtualBox

- Create two VMs: Windows Server 2022 and Windows 11.
- Set both network adapters to **Internal Network** (same network name, e.g., `intnet`).

### 2. Configure Static IP Addresses

**On Windows Server 2022:**
- Go to Control Panel → Network and Sharing Center → Change adapter settings.
- Set static IP: `192.168.10.1`, subnet mask `255.255.255.0`.
- Set preferred DNS server to `127.0.0.1` (or `192.168.10.1`).

**On Windows 11:**
- Set static IP: `192.168.10.2`, subnet mask `255.255.255.0`.
- Set preferred DNS server to `192.168.10.1`.

### 3. Install DNS Server Role on Windows Server 2022

- Open **Server Manager** → Add roles and features.
- Select **DNS Server** role and install.

### 4. Create Forward Lookup Zone

- Open **DNS Manager** (`dnsmgmt.msc`).
- Right-click **Forward Lookup Zones** → New Zone → Primary Zone.
- Zone name: `Area51.Local`.
- Accept default file creation → Finish.

### 5. Add DNS A Record (TestPC)

- In `Area51.Local`, right-click → **New Host (A or AAAA)**.
- Name: `TestPC`, IP address: `192.168.10.2`.
- Click **Add Host** → OK.

### 6. Create Reverse Lookup Zone (for PTR record)

- Right-click **Reverse Lookup Zones** → New Zone → Primary Zone.
- IPv4 Reverse Lookup Zone → Network ID: `192.168.10`.
- Finish zone creation.
- Add **PTR record** for `192.168.10.1` pointing to server's FQDN (e.g., `WIN-xxxx.Area51.Local`).

### 7. Configure Windows 11 DNS Suffix (to enable `ping TestPC`)

- On Windows 11, open **Control Panel** → Network adapter properties.
- IPv4 → Advanced → DNS tab.
- Add DNS suffix: `Area51.Local`.

### 8. Test and Verify

Open Command Prompt or PowerShell on Windows 11 and run the following commands.

## Testing & Results

### Forward Lookup

`nslookup TestPC.Area51.Local`

![Forward lookup](images/nslookup-forward-TestPC-Area51.png)

### Reverse Lookup

`nslookup 192.168.10.1`

![Reverse lookup](images/nslookup-reverse-192.168.10.1.png)

### PowerShell DNS Resolution

`Resolve-DnsName TestPC.Area51.Local`

![Resolve-DnsName](images/Resolve-DnsName-TestPC.png)

### Client DNS Configuration

`ipconfig /all` showing DNS server `192.168.10.1`

![ipconfig /all](images/ipconfig-all-DNS-192.168.10.1.png)

### Server Configuration – Forward Lookup Zone

DNS Manager showing `Area51.Local` zone with `TestPC` A record.

![Forward lookup zone](images/lab.local-Forward-Lookup-Zone-Records.png)

### TestPC A Record Properties

![TestPC A record properties](images/DNS-Manager-TestPC-A-record-properties.png)

### Reverse Lookup Zone – PTR Record

PTR record for `192.168.10.1`

![Reverse zone PTR](images/Reverse-Lookup-Zone-PTR-192.168.10.1.png)

## Troubleshooting

### Issue 1: `ping TestPC` returned "could not find host" even though `nslookup` worked

**Cause:** Windows 11 was not appending the `Area51.Local` suffix to single-name queries.

**Solution:** Added the DNS suffix `Area51.Local` in the advanced TCP/IP settings of the network adapter.

### Issue 2: `ping TestPC.lab.local` still failed after suffix was added

**Cause:** Windows DNS client cache or network stack needed resetting.

**Solution:** Ran `ipconfig /flushdns`, `netsh winsock reset`, `netsh int ip reset`, then rebooted the VM.

### Issue 3: Could not create PTR record – reverse zone already existed

**Solution:** Used the existing reverse lookup zone and added the PTR record manually.



## Conclusion

The DNS server on Windows Server 2022 successfully resolves `TestPC.Area51.Local` to `192.168.10.2`. Both forward and reverse lookups are functional, proving the DNS role is correctly configured in the isolated VirtualBox lab environment.
