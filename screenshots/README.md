# Windows Server Lab Evidence

This directory documents my hands-on Windows Server 2025 and Hyper-V lab. The screenshots show the virtual machine configuration, Active Directory domain, DNS configuration, and domain-joined Windows clients.

## 1. Hyper-V Virtual Machine Overview

The Hyper-V environment contains Windows Server 2025 and Windows client virtual machines used for Windows infrastructure administration exercises.

![Hyper-V virtual machine overview](hyper-v-overview.png)

## 2. Windows Server Virtual Machine Settings

The screenshot documents the hardware and configuration settings of the Windows Server 2025 virtual machine.

![Windows Server VM settings](windows-server-vm-settings.png)

## 3. Server Manager

Server Manager displays the installed server roles and management tools, including Active Directory Domain Services and DNS.

![Server Manager dashboard](server-manager.png)

## 4. Local Server Configuration

The Local Server page shows the server name, the `lab.local` domain membership, and network configuration information.

![Local Server configuration](local-server.png)

## 5. Active Directory Domain

The lab uses the `lab.local` Active Directory domain to manage domain-related infrastructure.

### Domain Controllers

The Domain Controllers container shows the Windows Server domain controller.

![Active Directory domain controller](active-directory-domain-controllers.png)

An additional screenshot of the Domain Controllers container is also available.

![Additional domain controller view](active-directory-domain-controllers1.png)

### Domain-Joined Computers

The Computers container shows the Windows client machines registered in Active Directory: WIN7, WIN10, and WIN11.

![Active Directory computers](active-directory-computers.png)

## 6. DNS Configuration

DNS is configured for the `lab.local` domain.

### Forward Lookup Zones

The DNS Manager screenshot shows the available forward lookup zones, including `lab.local` and `_msdcs.lab.local`.

![DNS forward lookup zones](dns-forward-lookup-zones.png
