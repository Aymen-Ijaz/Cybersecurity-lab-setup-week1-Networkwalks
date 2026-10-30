# Cybersecurity-lab-setup-week1-Networkwalks

##  Project Overview
This project is the first phase of setting up a personal cybersecurity testing lab. The goal was to create an isolated virtual environment where I can safely practice ethical hacking and network testing without affecting my host machine's home network. 

I used **VirtualBox** as the hypervisor and installed **Kali Linux** as the attacking machine.

##  Tools & Technologies Used
*   **VirtualBox** (v7)
*   **Kali Linux** (Virtual Machine)
---

##  Lab Architecture & Setup
Based on the lab requirements, here is how the environment is configured:

*   **Network Subnet:** `10.0.0.0/24`
*   **Network Type:** NATNetwork (Custom)
*   **Kali Linux IP:** `10.0.0.2/24` (Static IP)
*   **Gateway:** `10.0.0.1`
*   **DNS:** `8.8.8.8` and `1.1.1.1`
*   **Features Enabled:**
    *   Bidirectional Clipboard & Drag/Drop
    *   Shared Folders (Host `Downloads` folder mapped to VM)
    *   Full Internet Access

---

##  Real Issues Faced & How I Fixed Them

Here are the real problems I encountered and how I solved them:

### 1. The "Virus Detected" False Alarm
*   **What happened:** When downloading the Kali Linux, my browser blocked the download, flagging it as a virus.
*   **Why it happened:** Kali Linux comes pre-installed with powerful security and hacking tools. Antivirus software often flags these tools as dangerous, even when the official download is completely safe.
*   **How I fixed it:** I verified I was on the official `kali.org` website, bypassed the browser warning, and added an exception in Windows Defender to allow the file to run.

### 2. The `VERR_FILE_NOT_FOUND` Error (VirtualBox)
*   **What happened:** When I first tried to start the Kali VM after importing it, VirtualBox aborted with a `VERR_FILE_NOT_FOUND` error.
*   **Why it happened:** The VirtualBox configuration file (`.vbox`) was imported, but it couldn't find the actual virtual hard disk (`.vdi` file) because the link between the settings and the disk file was broken.
*   **How I fixed it:** I located the `.vdi` file in my host machine's download folder, moved it into the correct VirtualBox VM folder, and manually re-linked the hard disk in VirtualBox under **Settings > Storage**.

### 3. "Connection Activation Failed" & Lost Internet
*   **What happened:** After attempting to set the static IP (`10.0.0.2`) using `nmcli`, the network connection dropped completely and gave a "Connection activation failed" error. 
*   **Why it happened:** The VirtualBox NAT router was blocking the IP due to **Duplicate Address Detection (DAD)**. NetworkManager was trying to "ping" the IP to check if it was free, but VirtualBox's internal services responded, causing Kali to panic and drop the connection.
*   **How I fixed it:** 
    1. Recreated the connection profile strictly bound to the `eth0` interface.
    2. Ran the PDF's fix to disable DAD: `sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0`
    3. Re-applied the static IP and brought the connection back up.

### 4. "Temporary Failure in Name Resolution" (DNS Bug)
*   **What happened:** The IP was set to `10.0.0.2`, but `ping google.com` failed with a DNS error, and `ping 8.8.8.8` gave a "Network Unreachable" error.
*   **Why it happened:** The connection was missing a **Default Gateway** to route traffic to the internet, and Kali didn't have DNS servers configured to translate domain names.
*   **How I fixed it:** I used `nmcli` to manually add the gateway (`10.0.0.1`) and the Google/Cloudflare DNS servers (`8.8.8.8` and `1.1.1.1`), then restarted the connection.

### 5. The Final Safety: Snapshots
*   Once everything was working perfectly (Static IP, Internet, and DNS), I powered off the VM and took a **Snapshot** named `Phase 1 Complete`. 
*   *Why this matters:* If I accidentally break the Linux OS while practicing hacking in the future, I can instantly restore the VM back to this exact working state without reinstalling everything.

---
### Proof of Working Network Configuration

<img width="846" height="560" alt="Screenshot 2026-10-30 201715" src="https://github.com/user-attachments/assets/61b3d852-9b44-464d-bf54-6c587b5bb53f" />
<img width="624" height="575" alt="Screenshot 2026-10-30 201806" src="https://github.com/user-attachments/assets/c72146cd-2616-4150-afbb-e64843857952" />
<img width="808" height="221" alt="Screenshot 2026-10-30 201837" src="https://github.com/user-attachments/assets/91aee925-4a5a-46a2-90c1-4093cc2c068d" />

---

