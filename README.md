# MSI-B550M-Pro-VDH-OpenCore
 
### Before you give this EFI a try, make sure you read [this](#set-cpu-core-count) and [this](#generating-your-own-serial-and-editing-rom)!

This repo includes an OpenCore EFI for the MSI B550M-Pro-VDH motherboard.

Testing on:

Model | MSI B550M-Pro-VDH
------------- | ---------------
CPU | AMD Ryzen 5 5600G
GPU | AMD Radeon RX 6600
RAM | 16 GB DDR4-3200 (Corsair Vengance)
Disk (NVMe) | Samsung 970 EVO
Ethernet | Realtek RTL8111H + RTL8125B (PCIe x1)

## What works?

- Boot
- USB
- Ethernet
- GPU acceleration
- Sleep

## What doesn't work?

- internal audio (not configured, since I use USB audio anyway)

## Set CPU core count

Core Count patch needs to be modified to boot your system, if not using a 6 core CPU. Find the four `algrey - Force cpuid_cores_per_package` patches and alter the `Replace` value only.

Changing `BA000000 0000`/`BA000000 0090`* to `BA <CoreCount> 0000 0000`/`BA <CoreCount> 0000 0090`* substituting `<CoreCount>` with the hexadecimal value matching your physical core count.

**Note:** *The four different values reflect the patch for different versions of macOS. Be sure to change all four if you want to boot macOS 10.15 to macOS 26*

See the table below for the values matching your CPU Core Count.

| CoreCount | Hexadecimal |
|--------|---------|
|   4 Core  | `04` |
|   6 Core  | `06` |
|   8 Core  | `08` |
|   12 Core | `0C` |
|   16 Core | `10` |

  For example on a 6 Core CPU the `Replace` value would result in these replace values, `BA 06 0000 0000`/`BA 06 0000 0090` (default configuration in this EFI)

## BIOS settings

[Dortania BIOS Guide](https://dortania.github.io/OpenCore-Install-Guide/AMD/zen.html#amd-bios-settings)

## GPU specific settings

AMD RX 5000/6000 users can skip this portion. For AMD Polaris/Vega, remove 'agdpmod=ignore' from boot-args.

## Creating the installer

There are two ways you can make a USB installer:

1. Have a working install of macOS, download the Installer from the App Store, then make a bootable Installer with the `createinstallmedia` command
2. If you are using Windows, use [macrecovery.py](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/windows-install.html) from the offical OpenCore release package.

After you have created a bootable Installer, copy the EFI folder to the EFI partition of the installer drive and install as usual (GUID Partiton Map; APFS). After the installation, mount the EFI partition of the installed OS and copy the EFI folder to its partition.

## Generating your own serial and Editing ROM

Use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) to generate a serial for MacPro7,1

use [ProperTree](https://github.com/corpnewt/propertree) to manually enter the details in the following sections of the config (as shown in the video): (SystemSerialNumber, MLB, UUID and ROM)

https://user-images.githubusercontent.com/59102649/116117179-3ea51200-a6bc-11eb-8a18-a03f7bb5bf1d.mp4

## Credits

* [TECHNIKVERBOT](https://github.com/TECHNIKVERBOT) (for creating the EFI)
* [AMD-OSX](https://github.com/AMD-OSX/AMD_Vanilla) (for the kernel patches)
* [acidanthera](https://github.com/acidanthera) (for OpenCore and the kexts)
* [dortania](https://dortania.github.io/OpenCore-Install-Guide/) for their awesome guide
