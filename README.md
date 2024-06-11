<img src="https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Logos/OpenCore_with_text_Small.png" width="200" height="48">

# Hackintosh-OpenCore-HP-Z420

**Premade EFI of OpenCore bootloader for HP Z420 is here, running Ventura and Sonoma!**

## Current Version: [OpenCore 1.0.0 DEBUG](https://github.com/acidanthera/OpenCorePkg/releases/tag/1.0.0) with [OCLP 1.5.0](https://github.com/dortania/OpenCore-Legacy-Patcher/releases/tag/1.5.0) running macOS 14.5 with success! :D

This repository provides a complete "Plug-and-Play" EFI setup for the OpenCore bootloader, along with all necessary files to install and run macOS on an HP Z420.

[![Download](https://img.shields.io/badge/Download-v1.0.0-brightgreen)](https://github.com/mavethee/Hackintosh-OpenCore-EFI-HP-Z420/releases/download/1.0.0/EFI.zip)

## Table of Contents
- [Download](#download)
- [What about Sequoia?](#what-about-sequoia)
- [What's Required to Make It Boot?](#whats-required-to-make-it-boot)
- [Running macOS Ventura if you want](#ventura-notes)
- [Installation Guide](#installation)
- [Notes for macOS Monterey if you insist](#monterey-notes)

## Cloning the repository:

```sh
# Clone this repository:
git clone https://github.com/mavethee/Hackintosh-OpenCore-EFI-HP-Z420.git

# Navigate to cloned repository:
cd Hackintosh-OpenCore-EFI-HP-Z420

# Open cloned repository in Finder:
open .
```

OR 

`Code` -> `Downaload ZIP`

Extract, put `EFI` on your USB.

## What about Sequoia?
As last year, it's not a big jump between 14 and 15 like it was with Ventura which introduced AVX2 requirement.
Not much news, development for needed 3D acceleration is going strong.

![HP Z420 Sequoia Beta 1 Screenshot](Screenshots/HPZ420_SequoiaB1.png)

As always will attach any good news in upcoming sources :D

For Sonoma, keep scrolling ^^

Sources:
- [Sequoia Development branch of OCLP](https://github.com/dortania/OpenCore-Legacy-Patcher/compare/main...sequoia-development)
- [macOS Sequoia Development PR](https://github.com/dortania/OpenCore-Legacy-Patcher/pull/1137)

## What's Required to Make It Boot?

![HP Z420 Sonoma Screenshot](Screenshots/HPZ420_Sonoma.png)

1. To run Sonoma successfully, you need at least OpenCore 0.9.3+ (officially 0.8.3 for AVX2 machines). KDKless install is now possible! 🎉  **Avoid RSR updates as they don't work with Rosetta Cryptex.**

- If you face login sreen issues on 14.1+, stay on 14.0 or disable lockscren and set automatic login before updating to latest. (Seems to be something I've only personally faced, but noting for the record):

   `Apple logo` -> `System settings` -> `Users and Groups` -> `Automatic login`

   **FIXED:** Update to **macOS 14.4+** and Root Patch with latest OpenCore Legacy Patcher release (**1.4.2 fixed the issue, but latest is more than recommended**)

2. Native dGPUs with AVX2 support are recommended. For Polaris and Vega dGPUs, root patching is required, but Navi GPUs are not supported.

3. Lack of AVX2 requires CryptexFixup for macOS 13+.

4. For Metal 1 dGPUs (e.g., Kepler), disable `mediaanalysisd` using `revblock=media` in NVRAM settings.

5. For non-AVX2 CPUs, disable f16c sysctl reporting by adding `revpatch=16c` to NVRAM settings.

6. OCLP works with Ventura since 0.5.0+. for Sonoma, it's active development so latest OCLP 1.5.0 is recommended.

7. While Legacy Metal dGPUs work for most part, there are still some issues.

8. Follow OCLP preparation steps, including setting SIP to `0x308` and disabling `Apple Secure Boot`:

-  Set SIP to 0x803: `NVRAM` -> `Add` -> `7C436110-AB2A-4BBB-A880-FE41995C9F82` -> `csr-active-config` -> `03080000`

-  Disable Apple Secure Boot: `Misc` -> `Security` -> `SecureBootModel` -> `Disable`

-  Disable Signed DMGs loading: `Misc` -> `Security` -> `DmgLoading` -> `Any`

-  Reset NVRAM using `ResetNvramEntry.efi` in `EFI/OC/DRIVERS`.

9. Use [AMFIPass v.1.4.0](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Acidanthera/AMFIPass-v1.4.0-RELEASE.zip) and re-enable AMFI for Sonoma.

10. Install macOS 14, then in post install patch your system using [the latest version of OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases/).

Sources:
- [macOS Sonoma and OpenCore Legacy Patcher Support](https://github.com/dortania/OpenCore-Legacy-Patcher/issues/1076)
- [Preliminary support for macOS Sonoma](https://github.com/dortania/OpenCore-Legacy-Patcher/pull/1077)
- [Legacy Metal Graphics Support and macOS Ventura - Sonoma](https://github.com/dortania/OpenCore-Legacy-Patcher/issues/1008)
- [Idle panic on Metal 1 GPUs during face analysis](https://github.com/dortania/OpenCore-Legacy-Patcher/pull/1013)
- [Rapid Security Response with legacy Macs](https://github.com/dortania/OpenCore-Legacy-Patcher/issues/1019)
- [Resolve CoreGraphics.framework crashing](https://github.com/dortania/OpenCore-Legacy-Patcher/commit/c0825ed24e98688ff430c30324f11b5c41840b8a)
- [Currently unsupported hardware in Ventura](https://dortania.github.io/OpenCore-Legacy-Patcher/VENTURA-DROP.html#currently-unsupported-broken-hardware-in-ventura)
- ...and some of my observations o7

## Ventura Notes

![HP Z420 Screenshot](Screenshots/HPZ420_Ventura.png)

Current config is prepared for booting Sonoma, so if you want to run Ventura, it's not Monterey, it's basically all the same.

If you're not interested in macOS 13, skip this step, [Installation Guide](#installation) awaits if you read [What's Required to Make It Boot?](#whats-required-to-make-it-boot) closely :D

Not like you have much to read here.

## Monterey Notes

Hopefully, Monterey is next in queue after Big Sur so this will get removed soon yaay! :D

If you're not interested in macOS 12, skip this step, [Installation Guide](#installation) awaits if you read [What's Required to Make It Boot?](#whats-required-to-make-it-boot) closely :D

![HP Z420 Screenshot](Screenshots/HPZ420_Monterey.png)

### Current config is prepared for booting Sonoma, so if you want to run Monterey, **revert Ventura and Sonoma NOTE steps.**

0. Remove Sonoma and Ventura related kexts:
   - `EFI/OC/Kexts/CryptexFixup.kext` (Allows non-AVX2 systems to boot on macOS 13 and newer)
   - `EFI/OC/Kexts/KDKLessWorkaround.kext` (Ventura only, KDKless install for macOS 14 is not possible yet)

### For unsupported dGPU:

1. Set SIP to 0x802: `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> csr-active-config -> 02080000`

2. Disable Apple Secure Boot: `Misc -> Security -> SecureBootModel -> Disable`

3. Disable Signed DMGs loading: `Misc -> Security -> DmgLoading -> Any`

4. Reset NVRAM using `ResetNvramEntry.efi` in `EFI/OC/DRIVERS`.

5. (Optional) For auto root patching your unsupported dGPU, add `AutoPkgInstaller.kext` to your `EFI/OC/KEXTS` from [here](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Acidanthera/AutoPkgInstaller-v1.0.3-DEBUG.zip).

6. Flash your config.plist, reboot macOS, and launch OCLP.

7. Follow OCLP prompts and reboot.

## Installation

Internal USB 3.0 doesn't work, but USB 2.0 ports should function properly.

USB mapping is important, so consider using USBMap's QUICKSTART guide [here](https://github.com/corpnewt/USBMap/blob/master/README.md#quick-start).

For USB 3.0, consider using the [Inatek KT4004 PCIe expansion card](https://www.amazon.pl/Inateck-Karta-USB-porty-ExpresCard/dp/B00HJ1DULE?th=1), which has native support.

To install macOS successfully:

- Use EFI from the `EFI` folder (if USB detection issues persist, see the USB section at the end of this README).

- Avoid using the present SMBIOS, as it's likely invalid. Regenerate using [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) for working iServices.

## What Works?

- Ethernet
- Audio
- USB (except internal USB3 ports)
- iServices (iMessage, FaceTime, AppStore, iCloud, etc.)

## What Doesn't Work?

- Internal USB3 ports, use PCIe ones, keep in mind they wont be visable by BIOS.
- Sleep (Clicking sounds on wake-up attempt)
- Fan Monitoring (needs to be manually mapped in config.plist?)

## Credits

- [AMFIPass](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Acidanthera/AMFIPass-v1.4.0-RELEASE.zip)
- [AutoPkgInstaller](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Acidanthera/AutoPkgInstaller-v1.0.2-DEBUG.zip)
- [CryptexFixup](https://github.com/acidanthera/CryptexFixup/releases/)
- [FeatureUnlock](https://github.com/acidanthera/FeatureUnlock/releases/)
- [HibernationFixup](https://github.com/acidanthera/HibernationFixup/releases/)
- [IntelMausi](https://github.com/acidanthera/IntelMausi/releases/)
- [KDKLessWorkaround](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Misc/KDKlessWorkaround-v1.0.0-DEBUG.zip)
- [Lilu](https://github.com/acidanthera/Lilu/releases/)
- [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg/releases/)
- [OpenCanopy's resources](https://github.com/acidanthera/OcBinaryData)
- [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases/)
- [RestrictEvents](https://github.com/acidanthera/RestrictEvents/releases/)
- [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases/)
- [WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases/)
