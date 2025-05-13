# ⚙️ Lab Setup Guide

This section walks through setting up the full virtual lab environment using **VirtualBox**, including:

- 🐉 **Kali Linux** (Attacker)
- 🧑‍💻 **Windows 11** (Workstation Target)
- 🗂️ **Windows Server** (Domain Controller / File Server)
- 📊 **Splunk** (SIEM)

---

## 🧰 Requirements

- [VirtualBox](https://www.virtualbox.org/) (latest version)
- Minimum **100 GB** disk space
- At least **16 GB RAM**
- Internet access (for ISO downloads)
- VT-x/AMD-V enabled in BIOS (optional, but improves performance)

---

## 🖥️ Step 1: Install All VMs

---

### ✅ Kali Linux (Attacker VM)

**Download:**  
🔗 [https://www.kali.org/get-kali/#kali-virtual-machines](https://www.kali.org/get-kali/#kali-virtual-machines)  
Choose the **VirtualBox prebuilt image (.ova)**

**Steps:**
1. In VirtualBox: `File > Import Appliance`
2. Select the downloaded `.ova` file
3. Boot the VM and update:
   ```bash
   sudo apt update && sudo apt upgrade -y
````

---

### ✅ Windows 11 (Workstation Target)

**Download ISO:**
🔗 [https://www.microsoft.com/software-download/windows11](https://www.microsoft.com/software-download/windows11)

**Steps:**

1. Create a new VM: `New > Windows 11`
2. Set:

   * RAM: 4–8 GB
   * CPUs: 2+
   * Disk: 60+ GB
3. Go to `Settings > Storage` and attach the Windows 11 ISO under **Controller: SATA**
4. Boot and install

**Optional – Bypass TPM/Secure Boot:**

1. Press `Shift + F10` at the setup screen
2. Run:

   ```cmd
   regedit
   ```
3. Navigate to:
   `HKEY_LOCAL_MACHINE\SYSTEM\Setup`
4. Create a new key: `LabConfig`
5. Inside `LabConfig`, add these DWORD (32-bit) values:

   ```ini
   BypassTPMCheck = 1
   BypassRAMCheck = 1
   BypassSecureBootCheck = 1
   ```

---

### ✅ Windows Server (Domain Controller or File Server)

**Download ISO:**
🔗 [https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022)

**Steps:**

1. Create a new VM: `New > Windows Server 2022`
2. Set:

   * RAM: 4–8 GB
   * CPUs: 2
   * Disk: 60+ GB
3. Mount the ISO under `Settings > Storage`
4. Install **with Desktop Experience**

**Post-install Setup:**

* Set a static IP address
* Rename the computer
* Optionally, install roles via **Server Manager**:

  * ✅ Active Directory Domain Services
  * ✅ DNS Server
  * ✅ File Server

---

### ✅ Splunk (SIEM Platform)

**Download Splunk Free:**
🔗 [https://www.splunk.com/en\_us/download/splunk-enterprise.html](https://www.splunk.com/en_us/download/splunk-enterprise.html)

**Use Ubuntu Server as the host:**

**Ubuntu Server ISO:**
🔗 [https://ubuntu.com/download/server](https://ubuntu.com/download/server)

**Steps:**

1. Create a new VM: `New > Linux > Ubuntu (64-bit)`
2. Set:

   * RAM: 2–4 GB
   * Disk: 40+ GB
3. Install Ubuntu
4. After install, run:

   ```bash
   wget -O splunk.deb 'https://download.splunk.com/products/splunk/releases/9.2.0/linux/splunk-9.2.0-a7c60d41769f-linux-2.6-amd64.deb'
   sudo dpkg -i splunk.deb
   sudo /opt/splunk/bin/splunk start --accept-license
   ```

**Access Splunk in your browser:**
`http://<splunk-vm-ip>:8000`

---

## 🌐 Step 2: Network Configuration

Set all VMs to the same **Host-Only Adapter** or **Internal Network** in VirtualBox:

```text
Settings > Network > Adapter 1 > Attached to: Host-Only Adapter
```

This allows your VMs to communicate **internally without internet exposure**.

---

## 📦 Step 3: Post-Install Tasks

After all VMs are installed:

* ✅ Install **Sysmon** and **Winlogbeat** on Windows machines for advanced logging
* ✅ Install **Filebeat** or **Wazuh Agent** on Linux systems
* ✅ Start simulating attacks with Kali:

  * Port scanning
  * SMB enumeration
  * SSH brute-force
  * File access
* ✅ Forward logs into **Splunk**

  * Use Filebeat, Winlogbeat, or custom inputs
  * Create detection rules, dashboards, and alert queries


