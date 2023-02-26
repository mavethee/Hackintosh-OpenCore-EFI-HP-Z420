<img src="https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Logos/OpenCore_with_text_Small.png" width="200" height="48"/>

## Hackintosh-OpenCore-HP-Z420
EFI premade of OpenCore bootloader for HP-Z420 is here and it runs Ventura!

## Current version - OpenCore 0.8.9 DEBUG
Repository contains full ,,Plug-and-Play" EFI of OpenCore bootloader and
all needed files to install and run macOS on HP Z420!

https://github.com/acidanthera/OpenCorePkg/releases/tag/0.8.9

<img src="https://media.discordapp.net/attachments/321319496990326784/1065988767749505104/Zrzut_ekranu_2023-01-20_o_14.38.02.png">

# Ventura NOTE:

For macOS 13 use OpenCore 0.8.3+, latest is recommended.

!Avoid installing 13.2! RSR updates DO NOT WORK due Rosetta Cryptex!

To install Ventura, you need MacPro7,1 SMBIOS, use EFI from `MP7,1_InstallEFI` folder (if still won't detect your USBs properly, see USB section on bottom of the README).

Present in repo SMBIOS is not purchased Apple's device but for own sake, I don't advice you to use it. ...for own sake ;)

Use GenSMBIOS: https://github.com/corpnewt/GenSMBIOS for that to regenerate MacPro7,1

Post-Install, you can optionally use EFI from repo's main EFI folder with MacPro6,1, pure astetics related to more matching hardware ;)

Regenerate MacPro6,1 SMBIOS for working iServices on Trash can! :D

## What has been done and must be done to make it boot?

1. Natively supported dGPUs require AVX2 support so you are dependent only on Legacy Metal dGPU and OCLP!

-   https://github.com/dortania/OpenCore-Legacy-Patcher/issues/998#issuecomment-1166340370

    **Side note:** Polaris and Vega dGPUs DO work, but require root patching just like Legacy Metal ones, newer than Polaris or Vega won't work!

2. Lack of AVX2 instruction set requires more fun with macOS 13 installation, so be aware! You need CryptexFixup to even boot!

-   https://github.com/acidanthera/CryptexFixup

3. If you are using Metal 1 dGPU, e.g Kepler dGPUs, disable mediaanalysisd which requires Metal 2 feature set!

-   Add `revblock=media` to `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`

    !Remove on Metal 2 dGPUs!

4. OCLP now works with Ventura since 0.5.0+! (for Vega dGPUs required is 0.6.0, but again latest is highly reccomended!)

-   https://github.com/dortania/OpenCore-Legacy-Patcher/releases/

-   https://github.com/dortania/OpenCore-Legacy-Patcher/actions/workflows/build-app-wxpython.yml (for Vega dGPUs as 0.6.0 is not out yet!)

5. While Legacy Metal dGPUs work for most part, there are still some issues you might want to track:

    https://github.com/dortania/OpenCore-Legacy-Patcher/issues/1008#issue-1400530902

6. OCLP preparation (already applied, listing to actually teach you something):

-   SET SIP to 0x308:

`NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> csr-active-config` to `03080000`

-   Disable Apple Secure Boot: `Misc -> Security -> SecureBootModel` to `Disable` 

-   Disable Signed DMGs loading: `Misc -> Security -> DmgLoading` to `Any`

-   Disable AMFI (+Fix for Electron apps on 12.3+):  `amfi=0x80 ipc_control_port_options=0` to `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`

-   Reset NVRAM using `ResetNvramEntry.efi` in `EFI\OC\DRIVERS`

-   (Optional) For auto root patching your unsupported dGPU generate `AutoPkgInstaller.kext` and add it to your `EFI\OC\KEXTS`:

        https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Acidanthera/AutoPkgInstaller-v1.0.1-DEBUG.zip

-   Flash your config.plist, reboot macOS and launch OCLP,

7. Follow OCLP prompts and reboot.

**Done! GPU acceleration in Ventura is working!**

Sources:

* https://github.com/dortania/OpenCore-Legacy-Patcher/issues/998 

* https://github.com/dortania/OpenCore-Legacy-Patcher/issues/1008

* https://github.com/dortania/OpenCore-Legacy-Patcher/pull/1013

* https://github.com/dortania/OpenCore-Legacy-Patcher/issues/1019

# What works:
* Ethernet,
* Audio,
* USB (except internal USB3 ports),
* iServices (iMessage, FaceTime, AppStore, iCloud, etc.).

# What doesn't work:
* Internal USB3 ports,
* Sleep (Clicking sounds on wake-up attempt),
* Fan Monitoring (needs to be manually mapped in config.plist, I didn't figure it out yet)


# USB issues (need help and contributors):

Internal USB 3.0 doesnt work although USB 2.0 ones should work, USB mapping actually does something so uploading USB map kexts in case it does something with your machine! ^^

It's good old trash can again! If you have issues installing Ventura, switch to MacPro7,1 then switch back after macOS is installed! :D

But generally check this README anyways:

https://github.com/corpnewt/USBMap/blob/master/README.md#quick-start


For working USB 3.0 I used this PCIe expansion card (native support, no kexts needed), although keep figuring out why BIOS doesn't care about it, appears only on OS level:

https://www.amazon.pl/Inateck-Karta-USB-porty-ExpresCard/dp/B00HJ1DULE?th=1

# Monterey NOTE:

Current config is prepared for booting Ventura so if you want to run Monterey with supported dGPU, **revert Ventura NOTE steps.**

For unsupported dGPU, follow the steps below:

-   Remove Ventura related kexts:

        `EFI/OC/Kexts/CryptexFixup.kext`

        `EFI/OC/Kexts/KDKLessWorkaround.kext`

-   SET SIP to 0x802: `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> csr-active-config` to `02080000`

-   Disable Apple Secure Boot: `Misc -> Security -> SecureBootModel` to `Disable` 

-   Disable Signed DMGs loading: `Misc -> Security -> DmgLoading` to `Any`

-   Reset NVRAM using `ResetNvramEntry.efi` in `EFI\OC\DRIVERS`

-   (Optional) For auto root patching your unsupported dGPU - add `AutoPkgInstaller.kext` to your `EFI\OC\KEXTS`:

        https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Acidanthera/AutoPkgInstaller-v1.0.1-DEBUG.zip

- Flash your config.plist, reboot macOS and launch OCLP,

- Follow OCLP prompts and reboot.

**Done! GPU acceleration in Monterey is back again!**

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
