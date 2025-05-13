
#⚙️ Lab Setup Guide
This section walks through setting up the full virtual lab environment, including Kali Linux (Attacker), Windows 11 (Workstation), Windows Server (Domain Controller/File Server), and Splunk (SIEM) using VirtualBox.

🧰 Requirements
VirtualBox (latest version)

At least 100 GB disk space and 16 GB RAM recommended

Internet access (for ISO downloads)

Optional: Enable VT-x/AMD-V in BIOS for performance

🖥️ Step 1: Install All VMs
✅ 1. Kali Linux (Attacker VM)
Download:

https://www.kali.org/get-kali/#kali-virtual-machines

Choose the VirtualBox prebuilt image

Steps:

Import the Kali .ova file via VirtualBox (File > Import Appliance)

Boot the VM and update packages:

bash
Copy
Edit
sudo apt update && sudo apt upgrade -y
✅ 2. Windows 11 (Workstation Target)
Download ISO:

https://www.microsoft.com/software-download/windows11

Steps:

In VirtualBox: New > Windows 11

Allocate 4–8 GB RAM, 2+ CPUs, 60+ GB disk

Attach ISO under Settings > Storage > Controller: SATA

Boot and install Windows 11

Optional (to bypass TPM/Secure Boot):

Press Shift+F10 during setup

Run:

cmd
Copy
Edit
regedit
Create key LabConfig under HKEY_LOCAL_MACHINE\SYSTEM\Setup, then add DWORDs:

ini
Copy
Edit
BypassTPMCheck       = 1
BypassRAMCheck       = 1
BypassSecureBootCheck = 1
✅ 3. Windows Server (Domain Controller or File Server)
Download ISO:

https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022

Steps:

Create new VM: New > Windows Server 2022

Assign 4–8 GB RAM, 2 CPUs, 60+ GB disk

Mount the ISO under Settings > Storage

Install with Desktop Experience

After install:

Set static IP

Rename the computer

Optionally install roles via Server Manager:

Active Directory Domain Services

DNS

File Server

✅ 4. Splunk (SIEM)
Download Free Version:

https://www.splunk.com/en_us/download/splunk-enterprise.html

Use Ubuntu Server (recommended) as the Splunk host:

Download Ubuntu Server ISO:

https://ubuntu.com/download/server

Create VM: New > Linux > Ubuntu (64-bit)

Allocate 2–4 GB RAM, 40+ GB disk

Install Ubuntu, then install Splunk:

bash
Copy
Edit
# After install, SSH into the VM or use terminal
wget -O splunk.deb 'https://download.splunk.com/products/splunk/releases/9.2.0/linux/splunk-9.2.0-a7c60d41769f-linux-2.6-amd64.deb'
sudo dpkg -i splunk.deb
sudo /opt/splunk/bin/splunk start --accept-license
Access Splunk in browser:
http://<splunk-vm-ip>:8000

🌐 Step 2: Networking
Set all VMs to the same Host-Only Adapter or Internal Network:

Settings > Network > Adapter 1 > Attached to: Host-only Adapter

This ensures all VMs can talk to each other privately.

📦 Step 3: Next Steps
After installation:

Install Sysmon and Winlogbeat on Windows machines

Install Filebeat or Wazuh agent on Linux systems

Start simulating attacks using Kali (port scans, SMB enumeration, brute-force, etc.)

Forward logs into Splunk for correlation and dashboard creation


