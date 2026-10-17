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
