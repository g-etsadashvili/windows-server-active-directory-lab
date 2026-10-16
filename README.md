# Windows Server 2025 & Active Directory Lab

## Overview

This repository documents my Windows Server and Windows client lab developed as part of my IT infrastructure studies. The environment uses Microsoft Hyper-V to explore Windows virtualization, server configuration, Active Directory, and network infrastructure.

The lab is designed around a central Windows Server 2025 domain controller and multiple Windows client virtual machines.

## Lab Environment

**Hypervisor:** Microsoft Hyper-V

**Server operating system:** Windows Server 2025

**Virtual disk format:** VHDX

### Virtual Machines

| Virtual machine | Operating system | Purpose |
|---|---|---|
| DC - Windows Server 2025 | Windows Server 2025 | Central server and intended domain controller |
| Win7pro | Windows 7 Professional | Windows client lab |
| Win10 | Windows 10 | Windows client lab |
| Windows11 | Windows 11 | Windows client and deployment lab |

## 1. Hyper-V Virtualization

Configured a Windows Server 2025 virtual machine in Hyper-V.

Recorded configuration:

- Generation 1 virtual machine
- 4 GB assigned RAM
- 6 virtual CPUs
- VHDX virtual disk
- Virtual switch named `External-Lab`

The lab also included configuring virtual network adapters and investigating connectivity between the virtual machine and its virtual network.

## 2. Windows 11 Master Image

Worked with a Windows 11 master virtual machine and explored:

- VHDX virtual disks
- Sysprep and Windows generalization
- Secure Boot
- TPM 2.0
- Virtual machine preparation and configuration

These activities relate to preparing Windows installations for use in a virtualized lab.

## 3. Active Directory Domain Services

The intended architecture uses Windows Server 2025 as the central domain controller for a Windows domain.

Planned domain configuration:

- **Domain name:** `lab.local`
- **NetBIOS name:** `LAB`
- **Directory service:** Active Directory Domain Services (AD DS)
- **Name resolution:** DNS
- **Global Catalog:** Selected during the planned domain-controller configuration

### Domain Controller Setup and Troubleshooting

During the domain-controller promotion process, Windows reported that TCP/IP networking needed to be properly configured. The server's network adapter also displayed a disconnected state.

The troubleshooting work included investigating Hyper-V virtual switches, network adapters, and the server's intended static network configuration.

Intended IP configuration during troubleshooting:

- **IP address:** `192.168.100.10`
- **Subnet mask:** `255.255.255.0`
- **Preferred DNS server:** `192.168.100.10`

These values document the configuration being investigated; they do not establish that the network or domain controller was working successfully.

**Recorded status:** Domain-controller promotion and successful domain operation were not confirmed in the available lab notes.

## 4. Windows Client Virtual Machines

The lab includes Windows 7 Professional, Windows 10, and Windows 11 virtual machines.

The overall objective is to understand how Windows clients interact with centralized Windows Server infrastructure, including domain membership, identity management, and network services.

Domain joining and centralized management should be documented as completed only after the relevant configurations have been verified.

## 5. Klientoperativ Course Topics

The related course material covered additional Windows administration concepts:

- Windows 10 and Windows 11 installation
- Driver management
- MBR and GPT partitioning
- RAID configurations
- NTFS permissions
- BIOS and UEFI
- Secure Boot and TPM
- Windows PE and WIM images
- Windows networking fundamentals

These are course topics and are not all claimed as completed practical configurations in this repository.

## 6. Next Steps

The remaining work is to verify the server's network connectivity and establish the actual state of the Active Directory environment.

Further work can include:

- Completing domain-controller promotion
- Verifying DNS and domain functionality
- Joining Windows clients to the domain
- Testing centralized user and computer management
- Documenting successful configurations and troubleshooting results

## Purpose

This repository records my practical Windows infrastructure work, virtualization setup, configuration attempts, troubleshooting, and learning progress.

The objective is to develop practical skills relevant to Windows Server administration, Active Directory, and network administration.
