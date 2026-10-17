# Windows 7 Professional — Sysprep and WIM Imaging Lab

## Overview

This project documents the process of preparing, capturing, and deploying a Windows 7 Professional system image in a Microsoft Hyper-V virtualized environment.

The objective was to create a generalized Windows 7 Professional installation using Sysprep, capture the installation as a Windows Imaging Format (WIM) file from Windows PE, and deploy that image to a separate test virtual machine. The final deployment was verified by booting into Windows 7 Professional and confirming that the operating system was installed and functional.

This lab demonstrates practical experience with operating system imaging, virtual machine management, Windows deployment tools, disk partitioning, and boot configuration troubleshooting.

## Objectives

- Create a clone of a Windows 7 Professional virtual machine in Hyper-V.
- Prepare the cloned installation for imaging using Sysprep and the Generalize option.
- Boot the virtual machine into Windows PE.
- Attach and prepare an additional virtual hard disk for storing the captured image.
- Identify disks and volumes using command-line tools.
- Capture the generalized Windows installation into a WIM image using DISM.
- Create a clean test virtual machine and deploy the captured WIM image.
- Configure the deployed operating system's boot files.
- Verify that the deployed Windows 7 Professional installation boots successfully.

## Lab Environment

| Component | Description |
|---|---|
| Hypervisor | Microsoft Hyper-V |
| Source operating system | Windows 7 Professional |
| Image preparation | Sysprep |
| Deployment environment | Windows PE |
| Image format | Windows Imaging Format (WIM) |
| Image capture and application | DISM |
| Virtual disk format | VHDX |
| Target system | Separate Windows 7 Professional test VM |

## Workflow

### 1. Exporting and Importing the Virtual Machine

The existing Windows 7 Professional virtual machine was exported and imported into Hyper-V to create a separate clone for the imaging process. The clone allowed the imaging workflow to be performed without modifying the original virtual machine.

### 2. Preparing Windows 7 Professional with Sysprep

The cloned virtual machine was started, and its system properties were checked before running Sysprep.

Sysprep was executed with the **Generalize** option to prepare the Windows installation for reuse on another virtual machine. Generalization removes system-specific information so that the installation can be deployed as a reusable image.

After preparation, the virtual machine's network connection was disconnected before continuing with the imaging process.

### 3. Booting into Windows PE

A Windows PE ISO image was attached to the cloned virtual machine, which was then booted into the Windows PE command-line environment.

Windows PE provided a separate environment for capturing the Windows installation without running the operating system being captured.

### 4. Preparing Additional Storage

Initially, an additional virtual hard disk was needed to store the WIM image separately from the Windows installation.

A virtual hard disk was attached to the virtual machine. After rescanning the disks in Windows PE, the available disks and volumes were identified.

The destination volume was prepared by selecting the correct disk, creating or formatting the required volume, and assigning a drive letter. The disk and volume assignments were checked before proceeding to avoid accidentally modifying the Windows source partition.

### 5. Capturing the Windows Image

The Windows partition was identified and verified before the capture operation.

The Windows installation was captured into a WIM file using DISM. The resulting image was stored on the additional virtual disk rather than on the Windows partition being captured.

After the capture operation, the WIM file was checked to confirm that the image had been created successfully.

**Purpose:** This step produced a reusable image of the generalized Windows 7 Professional installation that could be applied to a separate virtual machine.

### 6. Creating the Target Test Virtual Machine

A separate virtual machine named `Win7pro-Image-Test` was created to test the captured image.

The target VM was configured with a virtual disk for the deployed operating system. Windows PE was attached so that the target disk could be prepared and the WIM image applied from the deployment environment.

The virtual disk containing the captured WIM was also made available to the target VM through the lab's virtual disk configuration.

### 7. Preparing the Target Disk

The target disk was identified in Windows PE before partitioning. Disk and volume information was reviewed to distinguish the target disk from the disk containing the WIM image.

The target disk was partitioned and formatted as required, and drive letters were assigned to the relevant volumes.

The destination was checked before applying the image to ensure that the WIM file was deployed to the intended virtual disk.

### 8. Deploying the WIM Image

The captured WIM image was applied to the prepared target Windows partition using DISM.

After the image application completed, the target VM was configured to boot from the deployed Windows installation. Boot configuration was checked and corrected where necessary.

### 9. First Boot and Verification

The target virtual machine was restarted and booted into the deployed Windows 7 Professional installation.

The successful login and final verification screenshots document that the deployed operating system was accessible and that the imaging and deployment workflow had completed.

The final checks included confirming the target VM's startup, logging into Windows, and investigating duplicate boot entries encountered during the process.

## Key Technical Skills Demonstrated

- Hyper-V virtual machine export, import, cloning, and configuration
- Windows 7 Professional system preparation with Sysprep
- Windows PE boot and command-line administration
- Virtual disk attachment and disk identification
- Disk partitioning, formatting, and drive-letter assignment
- Windows imaging and deployment using DISM
- WIM image creation and verification
- Operating system deployment to a separate virtual machine
- Boot configuration troubleshooting and deployment validation

## Screenshot Documentation

The screenshots are stored in the [`screenshots`](./screenshots/) directory. Each image documents a stage of the workflow, from exporting the original virtual machine through final verification of the deployed Windows 7 Professional installation.

The screenshots are numbered sequentially to follow the order of the lab procedure.

## Outcome

A generalized Windows 7 Professional installation was captured as a WIM image and deployed to a separate Hyper-V test virtual machine. The target system was booted and accessed successfully, providing practical experience with the Windows imaging and deployment workflow.

This project demonstrates an end-to-end operating system imaging process in a virtual lab, including preparation, capture, deployment, and verification.

## Screenshot Documentation

The following screenshots document the complete Windows 7 Professional imaging workflow, from exporting and cloning the virtual machine to capturing a generalized WIM image, deploying it to a clean test VM, and verifying the final installation.

### Phase 1 — Exporting and Cloning the Virtual Machine

![Step 1](./screenshots/1%20exporting%20windows%207%20pro.png)

![Step 2](./screenshots/2%20Importing%20virtual%20machine.png)

![Step 3](./screenshots/3%20importing%20virtual%20machine.png)

![Step 4](./screenshots/4%20importing%20virtual%20machine.png)

![Step 5](./screenshots/5%20importing%20virtual%20machine.png)

![Step 6](./screenshots/6%20importing%20virtual%20machine.png)

![Step 7](./screenshots/7%20importing%20virtual%20machine.png)

![Step 8](./screenshots/8%20importing%20virtual%20machine.png)

![Step 9](./screenshots/9%20Created%20Windows%207%20pro%20clone.png)

![Step 10](./screenshots/10%20entering%20Win7%20pro%20clone%20%28system%20properties%29.png)

### Phase 2 — Sysprep and Windows PE Preparation

![Step 11](./screenshots/11%20using%20sysprep%20on%20windows%207%20pro%20clone.png)

![Step 12](./screenshots/12%20executing%20systemprep%20with%20generalize%20option.png)

![Step 13](./screenshots/13%20systemprepr%20executing.png)

![Step 14](./screenshots/14%20disconnecting%20network%20on%20my%20win7pro%20clone.png)

![Step 15](./screenshots/15%20adding%20Windows%20PE%20iso%20file.png)

![Step 16](./screenshots/16%20entering%20WindPE%20shell%20mode%20on%20Win7pro%20clone.png)

### Phase 3 — Additional Storage and WIM Capture

![Step 17](./screenshots/17%20I%20have%20only%20one%20disk%20I%20need%20to%20add%20additional%20disk%20to%20store%20WIM%20file.png)

![Step 18](./screenshots/18%20adding%20additional%20hard%20drive%20for%20WIM.png)

![Step 19](./screenshots/19%20adding%20additional%20hard%20drive%20for%20WIM.png)

![Step 20](./screenshots/20%20adding%20additional%20hard%20drive%20for%20WIM.png)

![Step 21](./screenshots/21%20adding%20additional%20hard%20drive%20for%20WIM.png)

![Step 22](./screenshots/22%20adding%20additional%20hard%20drive%20for%20WIM.png)

![Step 23](./screenshots/23%20attaching%20hard%20disk%20to%20Win7pro%20clone.png)

![Step 24](./screenshots/24%20in%20WinPE%20I%20did%20rescan%20now%20I%20have%20two%20disks%20from%20which%20I%20can%20chose%20where%20to%20install%20WIM.png)

![Step 25](./screenshots/25%20selecting%20disk%2C%20formating%20it%20and%20assigning%20letter%20D.png)

![Step 26](./screenshots/26%20confirming%20that%20fragmentation%20was%20successful.png)

![Step 27](./screenshots/27%20formated%20the%20volume%202%20also%20for%20WIM%20file.png)

![Step 28](./screenshots/28%20Verify%20the%20Windows%20partition.png)

![Step 29](./screenshots/29%20Capturing%20the%20Windows%207%20pro%20clone%20image.png)

![Step 30](./screenshots/30%20verification%20that%20WIM%20was%20successful%20.png)

### Phase 4 — Creating the Clean Test Virtual Machine

![Step 31](./screenshots/31%20creating%20clean%20%20windows%207%20pro%20Image%20test%20from%20the%20winpro7%20clone.png)

![Step 32](./screenshots/32%20creating%20clean%20%20windows%207%20pro%20Image%20test%20from%20the%20winpro7%20clone.png)

![Step 33](./screenshots/33%20creating%20clean%20%20windows%207%20pro%20Image%20test%20from%20the%20winpro7%20clone.png)

![Step 34](./screenshots/34%20%20creating%20clean%20%20windows%207%20pro%20Image%20test%20from%20the%20winpro7%20clone.png)

![Step 35](./screenshots/35%20creating%20clean%20%20windows%207%20pro%20Image%20test%20from%20the%20winpro7%20clone.png)

![Step 36](./screenshots/36%20confirming%20Win7pro%20Image%20Test%20existance%20and%20fixing%20OS.png)

![Step 37](./screenshots/37%20adding%20WinPE%20to%20Win7pro%20Image%20Test%20so%20I%20could%20deploy%20WIM%20to%20it.png)

### Phase 5 — Deploying and Verifying the WIM Image

![Step 38](./screenshots/38%20deploying%20WIM%20on%20Win7pro%20Image%20Test.png)

![Step 39](./screenshots/39%20confirming%20that%20I%20added%20additional%20drive%20Win7Pro-ImageDestination%20where%20I%20have%20WIM%20file.png)

![Step 40](./screenshots/40%20Identifing%20the%20disks.png)

![Step 41](./screenshots/41%20verifying%20which%20disk%20needs%20to%20be%20partitioned%20.png)

![Step 42](./screenshots/42%20partitioning%20disk%20addigning%20letters%20to%20drive.png)

![Step 43](./screenshots/43%20sucessfully%20created.png)

![Step 44](./screenshots/44%20booting%20to%20windows%207%20pro%20Image%20Test.png)

![Step 45](./screenshots/45%20Logged%20into%20Win7pro%20Image%20Test.png)

![Step 46](./screenshots/46%20finding%20duplicate%20boot%20entries.png)

![Step 47](./screenshots/47%20proof%20of%20successful%20installation.png)
