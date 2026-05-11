# 🧪 Lab: Create a Domain Controller on Windows Server 2022 (VirtualBox)

## 📌 The Real Story

Right after installing Windows Server 2022, there was no **"Promote to domain controller"** option in Server Manager.  
I had to manually install Active Directory binaries. I didn't know the exact feature name, so I had to search for it first.

---

## 📋 What I Actually Did

### Step 1 – Find the correct AD feature name

In **Command Prompt (Admin)** , ran:

```cmd
dism /online /get-features | findstr "DirectoryServices"

Output: showed DirectoryServices-DomainController – that was the one I needed.
Step 2 – Install AD DS using the correct feature name
cmd

dism /online /enable-feature /featurename:DirectoryServices-DomainController /all

Step 3 – Install management tools
powershell

Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools

    Note: Install-WindowsFeature wasn't recognized at first – running DISM first fixed that.

Step 4 – Promote the server (now yellow flag appeared)

    Clicked notification flag → Promote this server to a domain controller

    Chose Add a new forest → domain name: SecMentalist.local

    Set Safe Mode Administrator password

    Ignored DNS delegation warning (unchecked)

    Clicked Install → server rebooted

✅ Result

Domain SecMentalist.local created.
❓ Why findstr?

I didn't know the exact feature name. Searching with findstr showed me DirectoryServices-DomainController was the correct one – not DirectoryServices-ADAM or others.
⚠️ Issues I saw (and ignored)
Issue	What I did
Didn't know the exact feature name	Used findstr to list and find it
Install-WindowsFeature not recognized	Ran DISM first – then it worked
"DNS delegation cannot be created"	Unchecked – safe to ignore
Yellow warnings	Clicked Install anyway
VM slow / stuck at 92.7%	Waited ~20 minutes – completed
🧠 Final Result

    ✅ Server is a Domain Controller

    ✅ Domain: SecMentalist.local

    ✅ Login: SECMENTALIST\Administrator

    ✅ Active Directory Users and Computers now appears in Server Manager → Tools

*Tested on Windows Server 2022 + VirtualBox 7.0*
