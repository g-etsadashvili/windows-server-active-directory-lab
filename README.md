
# Windows Server 2025 & Active Directory Lab

![Windows Server](https://img.shields.io/badge/Windows_Server-2025-0078D4?logo=windows&logoColor=white)
![Hyper-V](https://img.shields.io/badge/Hyper--V-Virtualization-0078D4?logo=windows&logoColor=white)
![Active Directory](https://img.shields.io/badge/Active_Directory-AD_DS-4472C4)
![Status](https://img.shields.io/badge/Status-In_Progress-orange)

A hands-on IT infrastructure lab documenting Windows virtualization, Windows Server configuration, Windows client deployment, and Active Directory setup and troubleshooting using Microsoft Hyper-V.

This repository records practical work, configuration attempts, technical observations, and lessons learned while developing skills in Windows Server and network administration.

## Table of Contents

- [Overview](#overview)
- [Lab Environment](#lab-environment)
- [Lab Architecture](#lab-architecture)
- [1. Hyper-V Virtualization](#1-hyper-v-virtualization)
- [2. Windows 11 Master Image](#2-windows-11-master-image)
- [3. Active Directory Domain Services](#3-active-directory-domain-services)
- [4. Windows Client Virtual Machines](#4-windows-client-virtual-machines)
- [5. Klientoperativ Course Topics](#5-klientoperativ-course-topics)
- [6. Next Steps](#6-next-steps)
- [Purpose](#purpose)

## Overview

The lab uses Microsoft Hyper-V to explore virtual machines, virtual networking, Windows Server 2025, and Windows client operating systems.

The intended environment includes a Windows Server 2025 virtual machine for domain services and separate Windows client virtual machines for administration and deployment practice.

**Current documentation status:** Active Directory promotion and successful domain operation have not been confirmed in the available lab notes. The repository distinguishes planned configurations from verified results.

## Lab Environment

| Component | Configuration |
|---|---|
| Hypervisor | Microsoft Hyper-V |
| Host operating system | Windows 11 |
| Server operating system | Windows Server 2025 |
| Virtual disk format | VHDX |
| Virtual switch | `External-Lab` |

### Virtual Machines

| Virtual machine | Operating system | Purpose |
|---|---|---|
| DC - Windows Server 2025 | Windows Server 2025 | Intended domain controller |
| Win7pro | Windows 7 Professional | Windows client and imaging lab |
| Win10 | Windows 10 | Windows client lab |
| Windows11 | Windows 11 | Windows client and deployment lab |

## Lab Architecture

The intended architecture consists of a Windows 11 Hyper-V host, a virtual switch, a Windows Server virtual machine, and Windows client virtual machines.

```text
Physical Computer
└── Windows 11 Host
    └── Microsoft Hyper-V
        ├── External-Lab Virtual Switch
        ├── DC - Windows Server 2025
        │   ├── Intended role: Domain Controller
        │   ├── Active Directory Domain Services
        │   └── DNS
        ├── Win7pro
        ├── Win10
        └── Windows11
```

**Note:** This diagram represents the intended lab design, not proof of successful network connectivity, domain-controller promotion, or DNS operation.

## 1. Hyper-V Virtualization

Configured a Windows Server 2025 virtual machine in Microsoft Hyper-V.

### Recorded Configuration

- **Generation:** 1
- **Assigned memory:** 4 GB
- **Virtual processors:** 6
- **Virtual disk:** VHDX
- **Virtual switch:** `External-Lab`

The lab also involved configuring virtual network adapters and investigating connectivity between the virtual machine and its virtual network.

## 2. Windows 11 Master Image

Worked with a Windows 11 master virtual machine and explored:

- VHDX virtual disks
- Sysprep and Windows generalization
- Secure Boot
- TPM 2.0
- Virtual machine preparation and configuration

These activities relate to preparing Windows installations for use in a virtualized environment.

**Documentation note:** This section covers the Windows 11 master-image work. The separate Windows 7 Professional Sysprep and WIM imaging workflow should be documented independently to keep the two workflows distinct.

## 3. Active Directory Domain Services

The intended design uses Windows Server 2025 as the central domain controller for a Windows domain.

### Planned Domain Configuration

- **Domain name:** `lab.local`
- **NetBIOS name:** `LAB`
- **Directory service:** Active Directory Domain Services (AD DS)
- **Name resolution:** DNS
- **Global Catalog:** Selected during the planned domain-controller configuration

These values describe the intended configuration and should not be interpreted as confirmation that the domain was successfully established.

### Domain Controller Setup and Troubleshooting

During the domain-controller promotion process, Windows reported that TCP/IP networking needed to be properly configured. The server's network adapter also displayed a disconnected state.

Troubleshooting involved investigating:

- Hyper-V virtual switches
- Virtual network adapters
- Network connectivity
- The intended static IP configuration

### Intended Network Configuration

| Setting | Intended value |
|---|---|
| IP address | `192.168.100.10` |
| Subnet mask | `255.255.255.0` |
| Preferred DNS server | `192.168.100.10` |

These values document the configuration under investigation. They do not establish that the network or domain controller was working successfully.

**Recorded status:** Domain-controller promotion and successful domain operation were not confirmed in the available lab notes.

## 4. Windows Client Virtual Machines

The lab includes Windows 7 Professional, Windows 10, and Windows 11 virtual machines.

The learning objectives include understanding how Windows clients interact with centralized Windows infrastructure, including:

- Domain membership
- User and computer identity management
- DNS and network services
- Centralized administration

Domain joining and centralized management should be documented as completed only after the relevant configurations have been verified.

## 5. Klientoperativ Course Topics

The related course material covered the following Windows administration topics:

- Windows 10 and Windows 11 installation
- Driver management
- MBR and GPT partitioning
- RAID configurations
- NTFS permissions
- BIOS and UEFI
- Secure Boot and TPM
- Windows PE and WIM images
- Windows networking fundamentals

These topics represent course material and learning areas. They are not all claimed as completed practical configurations in this repository.

## 6. Next Steps

The next steps are to verify the server's network configuration and establish the actual state of the Active Directory environment.

Planned work includes:

- Verifying virtual network adapter connectivity
- Confirming the server's IP configuration
- Completing domain-controller promotion, if not already completed
- Verifying DNS and domain functionality
- Joining Windows clients to the domain
- Testing centralized user and computer management
- Recording successful tests and troubleshooting results

## Purpose

This repository documents practical Windows infrastructure work, virtualization setup, configuration attempts, troubleshooting, and learning progress.

The objective is to develop practical skills relevant to Windows Server administration, Active Directory, virtualization, and network administration.
