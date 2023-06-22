<img src="https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Logos/OpenCore_with_text_Small.png" width="200" height="48"/>

## Hackintosh-OpenCore-HP-Z420
EFI premade of OpenCore bootloader for HP-Z420 is here and it runs Ventura!

## Current version - OpenCore 0.9.3 DEBUG
Repository contains full ,,Plug-and-Play" EFI of OpenCore bootloader and
all needed files to install and run macOS on HP Z420!

https://github.com/acidanthera/OpenCorePkg/releases/tag/0.9.3

<img src="https://media.discordapp.net/attachments/321319496990326784/1065988767749505104/Zrzut_ekranu_2023-01-20_o_14.38.02.png">

## Sonoma NOTE (for Ventura and older, skip to Installation section):

<img src="https://media.discordapp.net/attachments/724306793819275309/1118201389953331270/HPZ420.png">

(Reference image has no acceleration as it runs the worst possible choice, 3802-based Metal GPU, which is Kepler)

To run Sonoma succesfully you need at least OpenCore 0.9.3+! (Officially 0.8.3, but that only refers to AVX2 machines, again latest release is desired anyways!)

!REMEMBER TO DISABLE AMFI AS `AMFIPass` PROBABLY DOESN'T WORK!

By making sure that `amfi=0x80 ipc_control_port_options=0` is set in `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`!

At current state the best solution for running macOS 14 Sonoma would be getting a Polaris or Vega GPUs! (Navi GPUs DO NOT WORK with OCLP!)

Despite being natively supported you should use OCLP and booting from VESA mode:

`NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args -> amd_no_dgpu_accel`

This will disable dGPU acceleration and allow you to boot into macOS 14 Sonoma!

Root Patch installed macOS 14 (DO NOT INSTALL FROM MODIFIED USB BY OCLP, IN CASE OF ANY FUCKUP!) using Sonoma branch:

https://github.com/dortania/OpenCore-Legacy-Patcher/actions/workflows/build-app-wxpython.yml?query=branch%3Asonoma-development

(Pick latest one -> scroll down to articafts -> download ZIP file)

Follow OCLP prompts and reboot!

Sources:

https://github.com/dortania/OpenCore-Legacy-Patcher/pull/1077 (I've included most fixes already, besides Polaris/Vega specifics as I don't own this GPU)

https://dortania.github.io/GPU-Buyers-Guide/misc/bootflag.html#amd-boot-arguments (AMD Boot Args deep-dive if something will go wrong)

Keep in mind, even tho Legacy AMD Polaris/Vega would be best choice right now, things are still in development for both sides.

THINGS MAY BREAK OR CONTAIN GRAPHICAL GLITCHES!

## Installation:

Internal USB 3.0 doesnt work although USB 2.0 ones should work, USB mapping actually does something so uploading USB map kexts in case it does something with your machine! ^^

https://github.com/corpnewt/USBMap/blob/master/README.md#quick-start

For working USB 3.0 I used this PCIe expansion card (native support, no kexts needed), although keep figuring out why BIOS doesn't care about it, appears only on OS level:

https://www.amazon.pl/Inateck-Karta-USB-porty-ExpresCard/dp/B00HJ1DULE?th=1

(for Monterey, just skip part below and scroll down to Monterey notes if you have Metal dGPU dropped in Monterey e.g Kepler)

To sucessfully perform macOS Ventura installation, you need MacPro7,1 SMBIOS! 

-   Use EFI from `MP7,1_InstallEFI` folder (if still won't detect your USBs properly, see USB section on bottom of the README).

    Present in repo SMBIOS is probably invalid/not purchased Apple's device but for own sake, I don't advice you to use it.

    Use GenSMBIOS: https://github.com/corpnewt/GenSMBIOS for that to regenerate MacPro7,1

Post-Install, you can optionally use EFI from repo's main EFI folder with MacPro6,1, pure astetics related to more matching hardware ;)

(As you may see on picture above, that's just my workaround over Apple's blacklist I guess, feel free to suggest how to do it without double EFIs),

## What has been done and must be done to make it boot?

0. For macOS 13 use OpenCore 0.8.3+, latest is recommended. !Avoid installing RSR updates! RSR updates DO NOT WORK due Rosetta Cryptex!

1. Natively supported dGPUs require AVX2 support so you are dependent only on Legacy Metal dGPU and OCLP!

-   https://github.com/dortania/OpenCore-Legacy-Patcher/issues/998#issuecomment-1166340370

    **Side note:** Polaris and Vega dGPUs DO work, but require root patching just like Legacy Metal ones, Navi won't work!

2. Lack of AVX2 instruction set requires more fun with macOS 13+ installation, so be aware! You need CryptexFixup to even boot!

-   https://github.com/acidanthera/CryptexFixup

3. If you are using Metal 1 dGPU, e.g Kepler dGPUs, disable mediaanalysisd which requires Metal 2 feature set!

-   Add `revblock=media` to `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`

    !Remove on Metal 2 dGPUs!

4. For non-AVX2 CPUs, you need to disable f16c sysctl reporting to resolve CoreGraphics.framework crashing or/and Safari rendering in macOS 13.3+:

-   Add `revpatch=16c` to `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`

5. OCLP now works with Ventura since 0.5.0+! (for 13.3+, use at least 0.6.7 (AMFIPass Beta Testing!) to fully address mentioned issues above!)

-   https://github.com/dortania/OpenCore-Legacy-Patcher/releases/tag/amfipass-beta-test

6. While Legacy Metal dGPUs work for most part, there are still some issues you might want to track:

    https://github.com/dortania/OpenCore-Legacy-Patcher/issues/1008#issue-1400530902

7. OCLP preparation (already applied, listing to actually teach you something):

-   SET SIP to 0x308: `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> csr-active-config` to `03080000`

-   Disable Apple Secure Boot: `Misc -> Security -> SecureBootModel` to `Disable` 

-   Disable Signed DMGs loading: `Misc -> Security -> DmgLoading` to `Any`

-   Disable AMFI (+Fix for Electron apps on 12.3+):  `amfi=0x80 ipc_control_port_options=0` to `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`

-   Reset NVRAM using `ResetNvramEntry.efi` in `EFI\OC\DRIVERS`

-   (Optional) For auto root patching your unsupported dGPU generate `AutoPkgInstaller.kext` and add it to your `EFI\OC\KEXTS`:

    https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Acidanthera/AutoPkgInstaller-v1.0.2-DEBUG.zip

-   Flash your config.plist, reboot macOS and launch OCLP,

8. Follow OCLP prompts and reboot. **Done! GPU acceleration in Ventura is working!**

9. (AMFIPass Beta-Testing):

-   Ensure you have AMFIPass.kext (included with repo, for now seems to be not obtainable aside OCLP, will update link later)

-   Re-enable AMFI (+Fix for Electron apps on 12.3+) by removing  `amfi=0x80 ipc_control_port_options=0` to `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`

-   Reboot and enjoy! :D

Sources:

* https://github.com/dortania/OpenCore-Legacy-Patcher/issues/998 

* https://github.com/dortania/OpenCore-Legacy-Patcher/issues/1008

* https://github.com/dortania/OpenCore-Legacy-Patcher/pull/1013

* https://github.com/dortania/OpenCore-Legacy-Patcher/issues/1019

* https://github.com/dortania/OpenCore-Legacy-Patcher/commit/c0825ed24e98688ff430c30324f11b5c41840b8a

* https://dortania.github.io/OpenCore-Legacy-Patcher/VENTURA-DROP.html#currently-unsupported-broken-hardware-in-ventura


## What works:
* Ethernet,
* Audio,
* USB (except internal USB3 ports),
* iServices (iMessage, FaceTime, AppStore, iCloud, etc.).

## What doesn't work:
* Internal USB3 ports,
* Sleep (Clicking sounds on wake-up attempt),
* Fan Monitoring (needs to be manually mapped in config.plist, I didn't figure it out yet)
# Monterey NOTE:

Current config is prepared for booting Ventura so if you want to run Monterey, **revert Ventura NOTE steps.**

0.   Remove Ventura related kexts:

    `EFI/OC/Kexts/CryptexFixup.kext`

    `EFI/OC/Kexts/KDKLessWorkaround.kext`

## For unsupported dGPU, follow the steps below (for natively supported dGPUs, please refer to default settings in Dortania's Guide):

1.   SET SIP to 0x802: 

    `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> csr-active-config -> 02080000`

2.   Disable Apple Secure Boot:

    `Misc -> Security -> SecureBootModel -> Disable` 

3.   Disable Signed DMGs loading:

    `Misc -> Security -> DmgLoading -> Any`

4.   Reset NVRAM using `ResetNvramEntry.efi` in `EFI\OC\DRIVERS`

5.   (Optional) For auto root patching your unsupported dGPU - add `AutoPkgInstaller.kext` to your `EFI\OC\KEXTS`:

        https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Acidanthera/AutoPkgInstaller-v1.0.1-DEBUG.zip

6. Flash your config.plist, reboot macOS and launch OCLP,

7. Follow OCLP prompts and reboot. **Done! GPU acceleration in Monterey is back again!**

## Credits:
### AutoPkgInstaller:
https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Acidanthera/AutoPkgInstaller-v1.0.2-DEBUG.zip
### CryptexFixup:
https://github.com/acidanthera/CryptexFixup
### FeatureUnlock:
https://github.com/acidanthera/FeatureUnlock
### HibernationFixup:
https://github.com/acidanthera/HibernationFixup
### IntelMausi:
https://github.com/acidanthera/IntelMausi
### KDKLessWorkaround
https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Misc/KDKlessWorkaround-v1.0.0-DEBUG.zip
### Lilu:
https://github.com/acidanthera/Lilu/
### OpenCorePkg:
https://github.com/acidanthera/OpenCorePkg
### OpenCanopy's resources:
https://github.com/acidanthera/OcBinaryData
### OpenCore Legacy Patcher:
https://github.com/dortania/OpenCore-Legacy-Patcher
### RestrictEvents:
https://github.com/acidanthera/RestrictEvents
### VirtualSMC:
https://github.com/acidanthera/VirtualSMC
### WhateverGreen:
https://github.com/acidanthera/WhateverGreen
