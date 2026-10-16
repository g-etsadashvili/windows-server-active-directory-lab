# Windows Server 2025 & Hyper-V Lab

## Overview

This repository documents my practical work with Windows Server 2025, Hyper-V virtualization, Windows client virtual machines, and the Windows administration concepts covered in my Klientoperativ course.

The lab focuses on virtual machine configuration, Windows deployment, Active Directory fundamentals, and network troubleshooting.

## Lab Environment

- **Hypervisor:** Microsoft Hyper-V
- **Server operating system:** Windows Server 2025
- **Client systems:** Windows 10 and Windows 11 virtual machines
- **Planned Active Directory domain:** `lab.local`
- **Planned NetBIOS name:** `LAB`
- **Virtual disk format:** VHDX

## Practical Work

### 1. Hyper-V Virtualization

Configured a Windows Server 2025 virtual machine with the following settings:

- Generation 1 virtual machine
- 4 GB RAM
- 6 virtual CPUs
- VHDX virtual disk
- Virtual switch named `External-Lab`

### 2. Windows 11 Master Image

Worked with a Windows 11 master virtual machine and explored:

- VHDX-based virtual machine disks
- Sysprep and Windows generalization
- Secure Boot and TPM 2.0 configuration
- Preparing a master image for additional virtual machines

### 3. Active Directory Domain Controller Setup

Worked on configuring Windows Server 2025 as the first domain controller for a new forest.

Planned configuration:

- Domain: `lab.local`
- NetBIOS name: `LAB`
- DNS server role enabled
- Global Catalog enabled
- No DNS delegation

**Status:** Domain controller promotion encountered a network connectivity problem. The virtual network adapter appeared disconnected inside the guest operating system. The networking issue was investigated but was not confirmed as resolved in the recorded lab work.

### 4. Network Troubleshooting

Investigated Hyper-V networking while preparing the server for Active Directory deployment.

Areas examined included:

- Virtual switches and network adapters
- The `External-Lab` switch
- The Hyper-V Default Switch
- Static IPv4 configuration
- DNS configuration for the intended domain controller

Intended static configuration during troubleshooting:

- IP address: `192.168.100.10`
- Subnet mask: `255.255.255.0`
- Preferred DNS server: `192.168.100.10`

These values document the intended configuration, not a verified working network setup.

## Klientoperativ Course Topics

The course also covered Windows installation and administration concepts, including:

- Windows 10 and Windows 11 installation
- Driver management
- MBR and GPT partitioning
- RAID configurations
- NTFS permissions
- BIOS, UEFI, Secure Boot, and TPM
- Windows PE and WIM images
- Windows networking fundamentals

These topics are included as course learning areas and are not all claimed as completed practical configurations.

## Next Steps

- Resolve the Hyper-V guest network connectivity issue.
- Complete and verify Active Directory Domain Services installation.
- Verify DNS functionality for `lab.local`.
- Test domain-controller operation before documenting further domain-management activities.

## Purpose

This repository records practical Windows infrastructure work, technical troubleshooting, and lessons learned while developing skills relevant to IT infrastructure and network administration.
