# Windows Server 2025 Datacenter - Initial Setup & Configuration

**Project Goal:**  
To demonstrate the complete, hands-on deployment of a Windows Server 2025 Datacenter (Evaluation) virtual machine from scratch. This project showcases my ability to perform system installation, core configuration, network troubleshooting, and security hardening—laying the groundwork for advanced services like Active Directory.

## Environment
- **Hypervisor:** VMware Workstation 17 Pro
- **Guest OS:** Windows Server 2025 Datacenter Evaluation (Build 26100)
- **Host OS:** Windows 11 Professional
- **Host Network:** Hotel/Guest Wi-Fi (Client Isolation enabled – blocks physical device-to-device communication)

## Network Strategy
Due to the hotel Wi-Fi's **Client Isolation** policy (which prevents devices on the same network from communicating), I configured the VM using **NAT mode**. This approach creates an isolated virtual network between the host and the VM, bypassing physical network restrictions while maintaining full internet access for the guest OS.

| Configuration | Detail |
| :--- | :--- |
| **VM Network Mode** | NAT (VMnet8) |
| **Host Virtual Adapter** | VMware Network Adapter VMnet8 (`192.168.105.1`) |
| **VM IP Assignment** | Static IP (manually assigned to ensure consistency) |
| **VM IP Address** | `192.168.105.10` |
| **VM Subnet Mask** | `255.255.255.0` (/24) |
| **VM Default Gateway** | `192.168.105.1` (Host's VMnet8 adapter) |
| **VM DNS Server** | `192.168.105.1` (forwarding to Host) |

## Post-Installation Configuration (via `sconfig`)

After the base OS installation, I utilized the Windows Server `sconfig` utility (Server Configuration Tool) to apply essential "Day 1" configurations:

1.  **System Updates:** Changed Windows Update behavior to **Manual** to prevent unexpected auto-reboots during lab development.
2.  **Remote Desktop (RDP):** Enabled Remote Desktop to allow direct management from my host PC without relying on the VMware console.
3.  **Remote Management (WinRM):** Enabled remote management to prepare the server for future PowerShell automation.
4.  **Server Renaming:** Renamed the default hostname to **`ServerSecM`** to align with enterprise naming standards and simplify network identification.

## Verification & Validation

I performed a series of connectivity tests to ensure the server was fully operational. This process uncovered a classic host-firewall issue, which I systematically resolved.

### Phase 1: Initial Connectivity Test (Failure)
I configured the VM with a static IP (`192.168.105.10`, gateway `192.168.105.1`) and attempted to ping the gateway from the VM:

```cmd
ping 192.168.105.1

Result: Request timed out. (4 packets lost, 100% loss)
Phase 2: Inspect VM Configuration (Checking Layer 3)

I ran ipconfig inside the VM to verify the static IP settings were applied correctly:
cmd

ipconfig

Result:

    IPv4 Address: 192.168.105.10 ✅

    Subnet Mask: 255.255.255.0 ✅

    Default Gateway: 192.168.105.1 ✅

The VM's network stack was perfectly configured. The issue was not on the VM side.
Phase 3: Check Host Services & Adapters

I verified that the host's VMware virtual infrastructure was healthy:

    Restarted "VMware NAT Service" and "VMware DHCP Service" via services.msc on the host.

    Verified that "VMware Network Adapter VMnet8" on the host was enabled and set to obtain IP automatically.

Result: Pings still timed out. The services were healthy, but traffic was still being blocked.
Phase 4: The Culprit – Host Windows Firewall

I suspected the host's Windows Firewall was blocking ICMP traffic intended for the virtual NAT adapter.

To test this hypothesis, I temporarily disabled the Host PC's Windows Firewall (for all networks).

Result: The VM's ping 192.168.105.1 succeeded immediately with replies. This confirmed that the host's firewall was the sole blocker.
Phase 5: Implement Permanent Fix (Security-First)

Instead of leaving the host firewall disabled (which is unsafe), I re-enabled it and created a permanent exception:

    Turned the Host firewall back ON.

    Opened Windows Defender Firewall with Advanced Security on the host (wf.msc).

    Navigated to Inbound Rules.

    Enabled the rule "File and Printer Sharing (Echo Request - ICMPv4-In)" for the Private and Public profiles.

Final Result: The VM can now ping the gateway (192.168.105.1) and access the internet (ping 8.8.8.8 works) while the host firewall remains fully active and secure.
Troubleshooting & Lessons Learned
1. Hotel Wi-Fi Client Isolation

Issue: Bridged mode failed because the hotel network prevented the host and VM from seeing each other.
Resolution: Switched to NAT mode, which creates a private virtual network that bypasses physical network restrictions.
2. Host Firewall Blocking NAT Traffic (Key Takeaway)

Issue: Even with the VM correctly configured on NAT, the Host PC's Windows Firewall dropped all ICMP packets destined for the virtual VMnet8 adapter.
Diagnosis: Restarted VMware services (no effect). Temporarily disabled the host firewall to prove the block, then re-enabled it.
Resolution: Permanently enabled the specific "Echo Request - ICMPv4-In" inbound rule on the host. This allows ping traffic for the virtual network without compromising overall host security.
Skills Demonstrated

    Virtualization: Deployment and networking (NAT vs. Bridged) in VMware Workstation.

    Windows Server Administration: Expert-level configuration of OS settings, updates, remote access, and system naming using sconfig and PowerShell.

    Network Fundamentals: Static IP assignment, subnet masking (/24), gateway configuration, and DNS resolution.

    Firewall & Security: Advanced Windows Firewall management—creating exceptions for ICMP without disabling protection, balancing accessibility with security.

    Systematic Troubleshooting: Layer-by-layer approach to diagnosing connectivity issues (VM config → Host Services → Firewall → Permanent Fix).
