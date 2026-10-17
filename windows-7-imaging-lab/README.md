# Windows 7 Imaging Lab — Hyper-V, WinPE & DISM

## Overview

This project documents a practical Windows 7 Professional SP1 virtual machine imaging workflow using Microsoft Hyper-V, Windows PE (WinPE), and DISM.

## Objectives

- Create an isolated clone of a Windows 7 virtual machine.
- Explore Sysprep and the Out-of-Box Experience (OOBE).
- Boot into WinPE and identify virtual disks and volumes.
- Capture a Windows installation into a WIM image using DISM.
- Verify the captured image metadata.
- Apply the WIM image to a target partition.
- Recreate boot files using BCDBoot.
- Verify that the deployed Windows installation boots successfully.

## Tools and Environment

- Microsoft Hyper-V
- Windows 7 Professional SP1
- Windows PE (WinPE)
- DISM
- DiskPart
- BCDBoot

## Workflow

1. Prepare and isolate the Windows 7 VM clone.
2. Configure Sysprep and OOBE.
3. Boot into WinPE.
4. Capture the Windows partition into a WIM file.
5. Verify the WIM image.
6. Apply the WIM image to the target partition.
7. Recreate boot files.
8. Boot and verify the deployed Windows installation.

## Important Limitation

The Sysprep Generalize option was not selected in this experiment. Therefore, this project does not claim that the installation was generalized or that its machine SID was changed.

## Results

The WIM capture and verification completed successfully. The image was applied successfully, boot files were created, and the deployed Windows 7 installation booted to the desktop.

## Evidence

Screenshots documenting the workflow will be stored in the `screenshots/` directory.
