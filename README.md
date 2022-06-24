<img src="https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Logos/OpenCore_with_text_Small.png" width="200" height="48"/>

## Hackintosh-OpenCore-HP-Z420
EFI premade of OpenCore bootloader for HP-Z420 is here!

## Current version - OpenCore 0.8.2 DEBUG (DEV) - REMEBER TO TRACK UPDATES ON DORTANIA BLOG TILL OFFICIAL RELEASE!
Repository contains full ,,Plug-and-Play" EFI of OpenCore bootloader and
all needed files to install and run macOS on HP Z420!

https://dortania.github.io/builds/

## USB issues (need help and contributors):

Internal USB 3.0 doesnt work although USB 2.0 ones should work, I have no idea why but since switched to MacPro7,1, USB mapping actually does something so uploading USB map kexts in case it does something with your machine! ^^ 

But generally check this README anyways:
https://github.com/corpnewt/USBMap/blob/master/README.md#quick-start


For working USB 3.0 I used this PCIe expansion card (native support, no kexts needed), although keep figuring out why BIOS doesn't care about it, appears only on OS level:
https://www.amazon.pl/Inateck-Karta-USB-porty-ExpresCard/dp/B00HJ1DULE?th=1

## Monterey NOTE:

Current config is cleared out from OCLP specific options so if you don't need OCLP to patch your dGPU, **you're good to go.**

Stay on MacPro7,1 for USB mapping sake as explained above.

But if you have Kepler or some other dropped dGPU you have to sacrifice a few things:

- No OTA updates or full updates at all due to Apple Secure Boot disabled and custom SIP on T2 SMBIOS

  **OR**

- Break USB Mapping with MacPro6,1 SMBIOS to have OTA updates despite Apple Secure Boot disabled and custom SIP on T2 SMBIOS

## OCLP preparation:

1. SET SIP to 0x802:
`NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> csr-active-config` to `02080000`

2. Disable Apple Secure Boot:
`Misc -> Security -> SecureBootModel` to `Disable` 

3. Reset NVRAM using `ResetNvramEntry.efi` in `EFI\OC\DRIVERS`

4. (Optional) For auto root patching your unsupported dGPU generate `AutoPkgInstaller.kext` and add it to your `EFI\OC\KEXTS`:

- Launch OCLP,

- Generate EFI for Mac that has something simular to your dGPU,

- Do not install generated EFI anywhere, copy temp location and get the kext!

- Flash config.plist with generated kext, reboot to check if it works.

Done! GPU acceleration in Monterey is back again! 

Downside is need to every time gather InstallerAssistants.pkg from e.g. MrMacintosh's blog:
https://mrmacintosh.com/macos-12-monterey-full-installer-database-download-directly-from-apple/

(Thats if you choose to stay with MacPro7,1!)

## Ventura NOTE - OCLP status, GPUs and why everything is enabled again...

OCLP DOESN'T WORK FOR MACOS 13, USE MACPRO7,1 SMBIOS ~~AND NATIVELY SUPPORTED DGPU!~~

New sad findings, even if natively supported dGPU will be used, you're out of luck as GPU accel require AVX2:

https://github.com/dortania/OpenCore-Legacy-Patcher/issues/998#issuecomment-1165304539

Memory mismatch error fix:

https://dortania.github.io/OpenCore-Post-Install/universal/memory.html

Or just use RestrictEvents included in Kexts.

Lack of AVX2 instruction set requires more fun with macOS 13 beta 2 installer, so be aware!
You need M1s dylds cache as its non AVX2 cache to boot successfully!

https://github.com/dortania/OpenCore-Legacy-Patcher/issues/998#issuecomment-1163607808

You can extract ipsw file from Apple Developer site or use link below:

https://updates.cdn-apple.com/2022SummerSeed/fullrestores/012-30346/9DD787A7-044B-4650-86D4-84E80B6B9C36/UniversalMac_13.0_22A5286j_Restore.ipsw

I've already applied mentioned patch for 'Root Hash verification' into config.plist but dylds with every update must be done manaully on your end.

Source: https://github.com/dortania/OpenCore-Legacy-Patcher/issues/998

<img src="https://cdn.discordapp.com/attachments/724306793819275309/989151977759989760/unknown.png">

(I used GT 640 so its pretty much unsupported as I mentioned above plus neofetch got confused since I installed it on DELL Optiplex 3050 and just swapped disks)

### SMBIOS:
Present in repo SMBIOS is not purchased Apple's device but for own sake, I don't advice you to use it.
...for own sake ;)

To generate SMBIOS you can use:
* GenSMBIOS:
https://github.com/corpnewt/GenSMBIOS
* OpenCore Auxiliary Tools:
https://github.com/ic005k/QtOpenCoreConfig

Tool doesn't matter really, you just need not valid or unused SMBIOS to copy-paste needed info.
...if you wish to use iServices of course :)

## Credits:

### Lilu:
https://github.com/acidanthera/Lilu/
### IntelMausi:
https://github.com/acidanthera/IntelMausi
### HibernationFixup:
https://github.com/acidanthera/HibernationFixup
### VirtualSMC:
https://github.com/acidanthera/VirtualSMC
### WhateverGreen:
https://github.com/acidanthera/WhateverGreen
### FeatureUnlock:
https://github.com/acidanthera/FeatureUnlock
### OpenCanopy's resources:
https://github.com/acidanthera/OcBinaryData
### OpenCore Legacy Patcher:
https://github.com/dortania/OpenCore-Legacy-Patcher
