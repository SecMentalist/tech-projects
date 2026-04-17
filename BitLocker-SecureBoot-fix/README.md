# “BitLocker TPM error after trying to encrypt a hard drive using the ‘Manage BitLocker’ control panel: ‘Unable to unlock the drive’ – fixed by enabling Secure Boot.”

## 📌 Problem

After turning on BitLocker for the C: drive (via **Manage BitLocker** control panel) and rebooting, Windows displayed the following error message:

> *“BitLocker could not be enabled: The Trusted Platform Module (TPM) was unable to unlock the drive. Either the system boot info changed after choosing BitLocker settings or the PIN did not match. If the problem persists after several tries, there may be a hardware or a firmware problem.”*

Encryption did not start. 

---

## 🔍 Diagnosis

1. Opened **System Information** (`msinfo32`).  
2. Checked **Secure Boot State** – it was **OFF**.  

This explained the TPM error: BitLocker with TPM‑only unlock requires Secure Boot to be enabled.

### Additional check: TPM version
- Ran `tpm.msc` to open the TPM Management console.  
- Confirmed TPM is **ready for use** and the specification version is **2.0** (required for full BitLocker functionality).

---

## 🛠️ Solution Steps

### Step 1: Enable Secure Boot
- Rebooted the PC and entered **BIOS / UEFI** settings.  
- Navigated to **Security** section.  
- Changed **Secure Boot** from `Disabled` to **Enabled**.  
- Saved changes and exited.

### Step 2: Enable Device Encryption (Windows Security)
- Opened **Windows Security** → **Device security**.  
- Turned **Device Encryption** **ON**.  

*(This ensures the TPM is properly initialised and paired with Secure Boot.)*

### Step 3: Re‑run BitLocker setup
- Opened **Manage BitLocker** again.  
- Turned on BitLocker for the C: drive again (chose “Save to file” for recovery key).  

### Step 4: Verify encryption is running
- Opened **Command Prompt as Administrator**.  
- Ran:
  ```cmd
  manage-bde -status C:

Output showed:
text

Percentage Encrypted: 16.8% (and climbing)
Conversion Status: Encryption in Progress

    
### Step 5: Handle encryption pause 

The encryption progress stopped at 29.7% for several minutes and did not advance.

To restart it, I first paused (optional) and then resumed the encryption:
cmd

manage-bde -pause C:
manage-bde -resume C:

Then verified the progress was moving again:
cmd

manage-bde -status C:

The percentage began increasing past 29.7% and continued toward 100%.
✅ Outcome

    BitLocker encryption successfully started and progressed to 100% after resuming.

    Final status verified with manage-bde -status C::
    text

    Conversion Status:    Encryption Completed
    Percentage Encrypted: 100.0%
    Protection Status:    On
    Lock Status:          Unlocked

    
    Windows reboots normally without asking for a recovery key (TPM unlocks automatically).

    The recovery key is safely stored to a USB stick.