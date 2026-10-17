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

## Screenshot Documentation

This section documents the complete Windows 7 Professional imaging workflow in Hyper-V, from preparing the source VM and capturing the WIM image to deploying it on a clean test VM and verifying successful boot.

### 1. Virtual Machine Preparation

![1. Exporting Windows 7 Professional](./1%20exporting%20windows%207%20pro.png)

**Step 1 — Exporting Windows 7 Professional:** Preparing the original virtual machine for the imaging workflow.

![2. Importing virtual machine](./2%20Importing%20virtual%20machine.png)

**Step 2 — Importing the virtual machine:** Importing the VM into Hyper-V.

![3. Importing virtual machine](./3%20importing%20virtual%20machine.png)

**Step 3 — Importing virtual machine:** Continuing the VM import process.

![4. Importing virtual machine](./4%20importing%20virtual%20machine.png)

**Step 4 — Importing virtual machine:** Further import configuration.

![5. Importing virtual machine](./5%20importing%20virtual%20machine.png)

**Step 5 — Importing virtual machine:** Continuing the import workflow.

![6. Importing virtual machine](./6%20importing%20virtual%20machine.png)

**Step 6 — Importing virtual machine:** Import setup progress.

![7. Importing virtual machine](./7%20importing%20virtual%20machine.png)

**Step 7 — Importing virtual machine:** Completing the import workflow.

![8. Importing virtual machine](./8%20importing%20virtual%20machine.png)

**Step 8 — Importing virtual machine:** Final stage of the import process.

![9. Created Windows 7 Pro clone](./9%20Created%20Windows%207%20pro%20clone.png)

**Step 9 — Creating the clone:** Creating the Windows 7 Professional clone used for imaging.

### 2. Sysprep Preparation

![10. Entering Windows 7 Pro clone system properties](./10%20entering%20Win7%20pro%20clone%20%28system%20properties%29.png)

**Step 10 — System properties:** Inspecting the cloned Windows 7 Professional system.

![11. Using Sysprep on Windows 7 Pro clone](./11%20using%20sysprep%20on%20windows%207%20pro%20clone.png)

**Step 11 — Sysprep:** Opening and preparing Sysprep on the cloned system.

![12. Executing Sysprep with the Generalize option](./12%20executing%20systemprep%20with%20generalize%20option.png)

**Step 12 — Generalize option:** Configuring Sysprep with the Generalize option.

![13. Sysprep executing](./13%20systemprepr%20executing.png)

**Step 13 — Sysprep execution:** Running the system preparation process.

![14. Disconnecting the network on the Windows 7 Pro clone](./14%20disconnecting%20network%20on%20my%20win7pro%20clone.png)

**Step 14 — Network disconnection:** Disconnecting the clone from the network before continuing.

### 3. Windows PE and Storage Preparation

![15. Adding the Windows PE ISO file](./15%20adding%20Windows%20PE%20iso%20file.png)

**Step 15 — Windows PE ISO:** Attaching the Windows PE ISO to the virtual machine.

![16. Entering Windows PE shell mode](./16%20entering%20WindPE%20shell%20mode%20on%20Win7pro%20clone.png)

**Step 16 — Windows PE shell:** Booting into the Windows PE environment.

![17. Identifying the need for an additional disk](./17%20I%20have%20only%20one%20disk%20I%20need%20to%20add%20additional%20disk%20to%20store%20WIM%20file.png)

**Step 17 — Storage planning:** Identifying the need for a separate disk to store the WIM image.

![18. Adding an additional hard drive](./18%20adding%20additional%20hard%20drive%20for%20WIM.png)

**Step 18 — Adding storage:** Configuring an additional virtual hard drive.

![19. Adding an additional hard drive](./19%20adding%20additional%20hard%20drive%20for%20WIM.png)

**Step 19 — Additional storage:** Continuing the virtual disk configuration.

![20. Adding an additional hard drive](./20%20adding%20additional%20hard%20drive%20for%20WIM.png)

**Step 20 — Additional storage:** Further configuration of the WIM destination disk.

![21. Adding an additional hard drive](./21%20adding%20additional%20hard%20drive%20for%20WIM.png)

**Step 21 — Additional storage:** Continuing the disk setup.

![22. Adding an additional hard drive](./22%20adding%20additional%20hard%20drive%20for%20WIM.png)

**Step 22 — Additional storage:** Completing the additional disk setup.

![23. Attaching the hard disk to the Windows 7 Pro clone](./23%20attaching%20hard%20disk%20to%20Win7pro%20clone.png)

**Step 23 — Attaching the disk:** Connecting the additional virtual hard drive to the clone.

![24. Rescanning disks in Windows PE](./24%20in%20WinPE%20I%20did%20rescan%20now%20I%20have%20two%20disks%20from%20which%20I%20can%20chose%20where%20to%20install%20WIM.png)

**Step 24 — Disk rescan:** Rescanning storage in Windows PE to detect both disks.

![25. Selecting and formatting the disk](./25%20selecting%20disk%2C%20formating%20it%20and%20assigning%20letter%20D.png)

**Step 25 — Disk preparation:** Selecting the destination disk, formatting it, and assigning drive letter D.

![26. Confirming successful formatting](./26%20confirming%20that%20fragmentation%20was%20successful.png)

**Step 26 — Formatting verification:** Checking the result of the disk preparation operation.

![27. Formatting the second volume for the WIM file](./27%20formated%20the%20volume%202%20also%20for%20WIM%20file.png)

**Step 27 — Destination volume:** Preparing the second volume for the WIM file.

![28. Verifying the Windows partition](./28%20Verify%20the%20Windows%20partition.png)

**Step 28 — Partition verification:** Identifying and verifying the Windows partition before capturing the image.

### 4. Capturing and Verifying the WIM Image

![29. Capturing the Windows 7 Pro clone image](./29%20Capturing%20the%20Windows%207%20pro%20clone%20image.png)

**Step 29 — Image capture:** Capturing the Windows 7 Professional clone into a WIM image.

![30. Verifying that the WIM capture was successful](./30%20verification%20that%20WIM%20was%20successful%20.png)

**Step 30 — WIM verification:** Checking that the captured image was created successfully.

### 5. Creating and Preparing the Clean Test VM

![31. Creating the clean Windows 7 Pro image test VM](./31%20creating%20clean%20windows%207%20pro%20Image%20test%20from%20the%20winpro7%20clone.png)

**Step 31 — Test VM preparation:** Starting the creation of a clean Windows 7 Professional image test VM.

![32. Creating the clean Windows 7 Pro image test VM](./32%20creating%20clean%20windows%207%20pro%20Image%20test%20from%20the%20winpro7%20clone.png)

**Step 32 — Test VM preparation:** Continuing the test VM creation process.

![33. Creating the clean Windows 7 Pro image test VM](./33%20creating%20clean%20windows%207%20pro%20Image%20test%20from%20the%20winpro7%20clone.png)

**Step 33 — Test VM preparation:** Further configuration of the clean test VM.

![34. Creating the clean Windows 7 Pro image test VM](./34%20creating%20clean%20windows%207%20pro%20Image%20test%20from%20the%20winpro7%20clone.png)

**Step 34 — Test VM preparation:** Continuing the virtual machine setup.

![35. Creating the clean Windows 7 Pro image test VM](./35%20creating%20clean%20windows%207%20pro%20Image%20test%20from%20the%20winpro7%20clone.png)

**Step 35 — Test VM preparation:** Completing the creation workflow for the image test VM.

![36. Confirming the test VM and fixing the OS](./36%20confirming%20Win7pro%20Image%20Test%20existance%20and%20fixing%20OS.png)

**Step 36 — Test VM verification:** Confirming the test VM exists and addressing the operating system setup.

![37. Adding Windows PE to the test VM](./37%20adding%20WinPE%20to%20Win7pro%20Image%20Test%20so%20I%20could%20deploy%20WIM%20to%20it.png)

**Step 37 — Windows PE setup:** Attaching Windows PE to the test VM to deploy the WIM image.

### 6. Deploying the WIM Image

![38. Deploying the WIM on the test VM](./38%20deploying%20WIM%20on%20Win7pro%20Image%20Test.png)

**Step 38 — Image deployment:** Deploying the captured WIM image to the Windows 7 Professional test VM.

![39. Confirming the WIM destination drive](./39%20confirming%20that%20I%20added%20additional%20drive%20Win7Pro-ImageDestination%20where%20I%20have%20WIM%20file.png)

**Step 39 — Destination verification:** Confirming the additional drive containing the WIM image is available.

![40. Identifying the disks](./40%20Identifying%20the%20disks%20.png)

**Step 40 — Disk identification:** Identifying the disks in the Windows PE environment.

![41. Verifying which disk needs partitioning](./41%20verifying%20which%20disk%20needs%20to%20be%20partitioned%20.png)

**Step 41 — Disk verification:** Confirming which disk needs to be partitioned.

![42. Partitioning the disk and assigning drive letters](./42%20partitioning%20disk%20addning%20letters%20to%20drive.png)

**Step 42 — Partitioning:** Partitioning the disk and assigning drive letters for deployment.

![43. Successfully created](./43%20successfully%20created.png)

**Step 43 — Creation confirmation:** Verifying the successful completion of the disk preparation step.

### 7. Boot Verification and Final Result

![44. Booting Windows 7 Pro Image Test](./44%20booting%20to%20windows%207%20pro%20Image%20Test.png)

**Step 44 — First boot:** Booting the deployed Windows 7 Professional test VM.

![45. Logged into Windows 7 Pro Image Test](./45%20Logged%20into%20Win7pro%20Image%20Test.png)

**Step 45 — Login verification:** Successfully logging into the deployed Windows 7 Professional test VM.

![46. Finding duplicate boot entries](./46%20finding%20duplicate%20boot%20entries.png)

**Step 46 — Boot configuration troubleshooting:** Identifying duplicate Windows boot entries.

![47. Proof of successful installation](./47%20proof%20of%20successful%20installation.png)

**Step 47 — Final verification:** Documenting the successful Windows 7 Professional installation and boot.
