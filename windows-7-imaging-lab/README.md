# Windows 7 Professional — Sysprep, WIM Capture and Deployment Lab

## Overview

This project documents the process of exporting and cloning a Windows 7 Professional virtual machine in Hyper-V, preparing the clone with Sysprep, capturing the generalized operating system into a Windows Imaging Format (WIM) image, and testing deployment in a separate virtual machine.

The objective was to understand the practical workflow for Windows image preparation, capture, deployment, and boot verification using Hyper-V, Windows PE, DiskPart, and DISM.

## Lab Environment

| Component | Configuration |
|---|---|
| Hypervisor | Microsoft Hyper-V |
| Source operating system | Windows 7 Professional SP1 |
| OS architecture | 32-bit |
| Windows build | 6.1, Build 7601 |
| Source VM | `Win7pro` |
| Cloned VM | `Win7pro Clone` |
| Deployment test VM | `Win7pro Image Test` |
| Windows PE media | `WinPE.iso` |
| Captured image | `Win7Pro.wim` |
| Image destination volume label | `WIM-DEST` |
| Image destination drive letter | `E:` in the capture environment |
| Deployment test disk | 32 GB dynamically expanding VHDX |

The source Windows 7 installation used a 25 GB virtual disk. A separate 10 GB virtual disk was created for storing the captured WIM image.

## 1. Exporting and Cloning the Virtual Machine

The original `Win7pro` virtual machine was exported through Hyper-V Manager.

The exported machine was imported using the **Import Virtual Machine** wizard.

Import configuration:

- Selected the exported `Win7pro` virtual machine.
- Chose **Copy the virtual machine (create a new unique ID)**.
- Configured the virtual machine files under `C:\Hyper-V\Win7Pro-Clone`.
- Completed the import and verified that `Win7pro Clone` appeared in Hyper-V Manager.

Creating a separate clone allowed the image preparation process to be performed independently of the original virtual machine.

## 2. Verifying the Windows Installation

The cloned virtual machine was started and its system information was checked.

The installation was identified as Windows 7 Professional with Service Pack 1, running as a 32-bit operating system.

The computer name displayed in the initial system properties was `Win7`.

## 3. Preparing the Clone with Sysprep

The Sysprep utility was opened inside the cloned Windows 7 installation.

The following options were selected:

- **System Cleanup Action:** Enter System Out-of-Box Experience (OOBE)
- **Generalize:** Enabled
- **Shutdown Options:** Shutdown

Sysprep was executed to generalize the Windows installation before capturing it as a reusable image.

The screenshots show Sysprep processing its generalize phase.

Generalization prepares Windows for deployment to another virtual machine by removing machine-specific system information as appropriate. OOBE allows Windows to go through its initial setup experience after deployment.

## 4. Preparing Windows PE

The network adapter of `Win7pro Clone` was disconnected in Hyper-V settings.

The Windows PE ISO was attached to the virtual machine's DVD drive:

`C:\Hyper-V\iso files\WindowsPE iso\WinPE.iso`

The virtual machine was booted into Windows PE, and the command prompt was used for disk preparation and image capture.

## 5. Creating an Additional Virtual Hard Disk

Initially, DiskPart showed only one available disk in the capture environment.

A separate virtual hard disk was therefore created in Hyper-V.

Configuration:

| Setting | Value |
|---|---|
| Virtual disk format | VHDX |
| Virtual disk type | Dynamically expanding |
| Virtual disk name | `New Virtual Hard Disk.vhdx` |
| Virtual disk size | 10 GB |
| Storage location | `C:\Hyper-V\Win7Pro-Clone` |

The new disk was attached to `Win7pro Clone` through its SCSI controller.

## 6. Preparing the Image Destination Disk

After attaching the additional disk, DiskPart was used to rescan the available storage devices.

Commands:

```cmd
diskpart
rescan
list disk
list volume
```

The environment showed two disks:

- Disk 0 — 25 GB, containing the Windows installation.
- Disk 1 — 10 GB, intended for WIM storage.

Disk 1 was selected and prepared:

```text
select disk 1
create partition primary
format fs=ntfs quick label=WIM-DEST
```

An attempt to assign drive letter `D:` failed because that letter was already in use by the DVD drive.

After inspecting the available volumes, the new partition was assigned drive letter `E:`.

The relevant commands were:

```text
list volume
select volume 2
assign letter=E
exit
```

The resulting volume was identified as NTFS with the label `WIM-DEST`.

## 7. Verifying the Windows Source Directory

Before capturing the image, the Windows installation directory was inspected:

```cmd
dir C:\Windows
```

The command displayed the contents of the Windows directory, confirming that the source directory was accessible in the capture environment.

## 8. Capturing the Windows 7 Image with DISM

The Windows installation was captured into a WIM file using DISM.

Command:

```cmd
dism /Capture-Image /ImageFile:E:\Win7Pro.wim /CaptureDir:C:\ /Name:"Windows 7 Professional" /Compress:Max /CheckIntegrity
```

Parameter explanations:

- `/Capture-Image` — captures a directory into a Windows image.
- `/ImageFile:E:\Win7Pro.wim` — specifies the destination WIM file.
- `/CaptureDir:C:\` — specifies the directory being captured.
- `/Name:"Windows 7 Professional"` — assigns a descriptive image name.
- `/Compress:Max` — uses maximum WIM compression.
- `/CheckIntegrity` — enables integrity checking for the image operation.

DISM reported that the capture operation completed successfully.

## 9. Verifying the Captured WIM

The captured image was inspected using:

```cmd
dism /Get-WimInfo /WimFile:E:\Win7Pro.wim
```

The output confirmed:

- Image index: `1`
- Image name: `Windows 7 Professional`
- Image size: `10,195,275,307` bytes
- Operation status: Successfully completed

This verified that the WIM file contained the expected Windows 7 Professional image metadata.

## 10. Creating the Deployment Test VM

A separate virtual machine named `Win7pro Image Test` was created in Hyper-V.

The configuration shown in the screenshots included:

- Generation 1
- 2048 MB memory
- Networking disconnected
- A 32 GB dynamically expanding VHDX
- Windows PE ISO attached to the DVD drive

The test VM was kept separate from the original source and the generalized capture VM so that the captured image could be tested independently.

## 11. Preparing the Deployment Disk

The deployment test VM was started in Windows PE.

DiskPart was used to inspect the disks and volumes. The 32 GB deployment disk was selected, and its partition was created and formatted for the Windows installation.

The deployment environment used these drive letters:

- `F:` — deployment Windows partition
- `E:` — `WIM-DEST` volume containing `Win7Pro.wim`
- `D:` — DVD-ROM drive containing Windows PE media

The drive letters are specific to the deployment environment and may differ from those used during capture.

## 12. Applying the WIM Image

The captured image was applied to the deployment partition using DISM:

```cmd
dism /Apply-Image /ImageFile:E:\Win7Pro.wim /Index:1 /ApplyDir:F:\ /CheckIntegrity
```

Parameter explanations:

- `/Apply-Image` — applies an image to a destination directory.
- `/ImageFile:E:\Win7Pro.wim` — specifies the captured WIM file.
- `/Index:1` — selects the first image in the WIM.
- `/ApplyDir:F:\` — specifies the destination partition.
- `/CheckIntegrity` — requests image integrity checking.

DISM reported:

`The operation completed successfully.`

This confirmed that the image application operation completed successfully.

## 13. Boot Configuration and Troubleshooting

After applying the image, the following command was attempted:

```cmd
bcdboot F:\Windows
```

The command returned:

`Failure when attempting to copy boot files.`

The initial boot-file creation attempt therefore failed.

The virtual machine was subsequently booted, and Windows Boot Manager displayed multiple Windows 7 entries.

After further investigation, the test machine successfully booted into Windows 7 Professional.

The installed edition was verified using the About Windows dialog, which showed Windows 7 Professional, Version 6.1 (Build 7601: Service Pack 1).

The boot configuration was also inspected using:

```cmd
bcdedit /enum
```

The output showed multiple Windows Boot Loader entries, consistent with the duplicate entries visible in Windows Boot Manager.

The screenshots document the investigation, but they do not establish that all duplicate entries were removed.

## 14. Final Result

The lab demonstrated the following workflow:

1. Exported a Windows 7 Professional virtual machine from Hyper-V.
2. Imported it as a separate clone with a new virtual machine ID.
3. Generalized the clone using Sysprep.
4. Booted into Windows PE.
5. Created and attached an additional virtual hard disk for image storage.
6. Prepared an NTFS destination volume using DiskPart.
7. Captured the Windows installation into `Win7Pro.wim` using DISM.
8. Verified the captured WIM metadata.
9. Created a separate deployment test virtual machine.
10. Applied the WIM to the deployment partition.
11. Booted into the deployed Windows 7 Professional installation.
12. Investigated boot-file creation errors and duplicate boot entries.

**Outcome:** The WIM image was captured successfully, its metadata was verified, and the image was applied to a separate virtual machine that subsequently booted into Windows 7 Professional.

## Skills Demonstrated

- Hyper-V virtual machine export and import
- Virtual machine cloning and unique IDs
- Sysprep and Windows generalization
- Windows PE
- Virtual disk creation and attachment
- DiskPart disk and volume management
- NTFS partition formatting and drive-letter assignment
- DISM image capture and deployment
- WIM metadata verification
- Windows Boot Manager and BCDEdit troubleshooting
- Documentation of configuration steps and troubleshooting results

## Project Scope

This repository section documents the **Windows 7 Professional imaging workflow only**.

Windows 11 Master Image work and Windows Server configuration are documented separately.


