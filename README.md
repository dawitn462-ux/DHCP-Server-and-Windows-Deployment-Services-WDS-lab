# 🌐 Active Directory Lab 2 — DHCP & Windows Deployment Services (WDS)

> **Windows Server 2022 | DHCP | WDS | PXE Network Deployment**  
> **Prepared by:** Dawit Nigus

![Windows Server](https://img.shields.io/badge/Windows_Server-2022-0078D4?logo=windows)
![Services](https://img.shields.io/badge/Services-DHCP_•_WDS-red)
![Deployment](https://img.shields.io/badge/Deployment-PXE-blue)

## 📌 Overview

This lab demonstrates how to configure **Dynamic Host Configuration Protocol (DHCP)** and **Windows Deployment Services (WDS)** on Windows Server 2022. DHCP automatically provides client network settings, while WDS enables Windows operating systems to be installed remotely over the network using **PXE (Pre-Boot Execution Environment)**.

The practical workflow covers installing the DHCP and WDS roles, creating and activating a DHCP scope, importing Windows 10 boot and install images, and deploying Windows 10 to a blank virtual machine without a USB drive or local installation DVD.

## 🎯 Objectives

- Install the DHCP Server role and authorize it in Active Directory.
- Create, configure, and activate a DHCP scope.
- Set the address pool, exclusions, lease duration, and DNS options.
- Install Windows Deployment Services (Deployment Server and Transport Server).
- Configure WDS as integrated with Active Directory.
- Add `boot.wim` and `install.wim` from Windows 10 installation media.
- PXE-boot a blank client VM and install Windows 10 over the network.
- Verify DHCP address assignment and successful WDS deployment.

## 🧪 Lab Environment

| Component | Configuration |
|---|---|
| Server VM | DC01 (VMware Workstation) |
| Server OS | Windows Server 2022 Datacenter Evaluation |
| Server hostname shown in lab | `WIN-FEVN8KFH14H` |
| Server resources | 4 GB RAM, 4 processors, 250 GB disk |
| Client VM | `Win 10 WDS` (blank VM) |
| Network | Both VMs on the same VMware LAN Segment |
| DHCP scope | `VLAN 1` |
| Network | `192.168.1.0/24` |
| DHCP pool | `192.168.1.1` – `192.168.1.254` |
| Exclusion | `192.168.1.1` – `192.168.1.10` |
| Lease duration | 1 hour |
| DNS server option | `192.168.1.200` |
| WDS remote install folder | `C:\RemoteInstall` |

## 📥 Required Downloads

Use official Microsoft and VMware/Broadcom sources:

1. **Windows Server 2022 Evaluation ISO** (for the server VM):  
   https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022

2. **Windows 10 ISO** (to extract `sources\boot.wim` and `sources\install.wim` for WDS):  
   https://www.microsoft.com/en-us/software-download/windows10  
   Microsoft may provide ISO download options depending on browser/device and current availability.

3. **VMware Workstation Pro** (Windows host):  
   https://support.broadcom.com/group/ecx/productdownloads?subfamily=VMware%20Workstation%20Pro

4. **Windows Server DHCP documentation**:  
   https://learn.microsoft.com/en-us/windows-server/networking/technologies/dhcp/dhcp-top

5. **Windows Deployment Services documentation**:  
   https://learn.microsoft.com/en-us/windows-server/administration/windows-deployment-services/get-started-with-wds

> **Image files used in this lab:** `D:\sources\install.wim` and `D:\sources\boot.wim` were selected from the mounted Windows 10 ISO. Drive letter `D:` may differ on your VM.

## 🛠️ Lab Workflow

### Part 1 — DHCP
1. Add the DHCP Server role in Server Manager.
2. Complete the DHCP post-install configuration and **authorize/commit** the server in Active Directory.
3. Open DHCP Manager and create a scope named `VLAN 1`.
4. Configure the pool `192.168.1.1–192.168.1.254` with mask `255.255.255.0`.
5. Exclude `192.168.1.1–192.168.1.10` from leases.
6. Set lease duration to 1 hour.
7. Configure DNS option `192.168.1.200` and activate the scope.

### Part 2 — WDS
1. Add the Windows Deployment Services role; keep **Deployment Server** and **Transport Server** selected.
2. Mount the Windows 10 ISO on the server VM.
3. Configure WDS as **Integrated with Active Directory**.
4. Keep the Remote Installation Folder at `C:\RemoteInstall`.
5. Set PXE response to **all client computers (known and unknown)** as in this lab.
6. If the WDS service times out during setup, start it manually from WDS Console → server → **All Tasks → Start**.
7. Add an install image group named `Win 10` and select `sources\install.wim`.
8. Add the boot image from `sources\boot.wim`.

### Part 3 — PXE Deployment
1. Create a blank client VM named `Win 10 WDS`.
2. Give it 4 GB RAM and 4 processors; connect it to the same LAN Segment as DC01.
3. Leave its virtual CD/DVD empty.
4. Power on and press `F12` (or `Fn + F12`) for network/PXE boot.
5. Obtain an address from DHCP, connect to WDS, authenticate, and select Windows 10 Enterprise Evaluation (x64).
6. Select the client disk and complete Windows Setup/OOBE.

## ✅ Expected Results

- DHCP scope `VLAN 1` is active and supplies addresses, excluding the reserved range.
- The client receives its IP configuration from DHCP.
- WDS offers the configured Windows 10 boot and installation images.
- A blank VM installs Windows 10 by PXE/network boot without USB/DVD installation media.

## 🧠 Key Concepts

- **DHCP:** Automatically leases IP configuration to clients.
- **Scope:** A configured address range and options for a subnet.
- **Exclusion:** Addresses DHCP must not lease.
- **Lease:** The time a client may use a DHCP address before renewal.
- **WDS:** Windows Server role for network-based Windows deployment.
- **PXE:** Network boot process used to start a client before its OS is installed.
- **`boot.wim`:** Starts Windows PE/Setup over the network.
- **`install.wim`:** Contains the Windows OS image that gets installed.

## 📚 Repository Contents

- `README.md` — Lab overview, setup workflow, and download links.
- `docs/AD-lab-2.pdf` — Full 44-page lab report with screenshots and assessment answers.
- `screenshots/` — Optional screenshots of DHCP, WDS, and PXE deployment.

## ⚠️ Lab Safety

Run this configuration only in an isolated, authorized virtual network. The DHCP scope and PXE response settings can affect other devices if connected to a shared network. Do not attach this practice DHCP server to a production LAN or an untrusted public network.

## 👨‍💻 Author

**Dawit Nigus**

*Automate addressing. Deploy over the network. Verify every stage.*
