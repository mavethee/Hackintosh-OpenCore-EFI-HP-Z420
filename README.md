<img src="https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Logos/OpenCore_with_text_Small.png" width="200" height="48"/>

## Hackintosh-OpenCore-HP-Z420
EFI premade of OpenCore bootloader for HP-Z420 is here and it runs Ventura!

## Current version - OpenCore 0.8.4 DEBUG
Repository contains full ,,Plug-and-Play" EFI of OpenCore bootloader and
all needed files to install and run macOS on HP Z420!

https://github.com/acidanthera/OpenCorePkg/releases/tag/0.8.4

<img src="https://media.discordapp.net/attachments/576381585310482443/1017518234729197698/Zrzut_ekranu_2022-09-8_o_21.32.22.png">
<img src="https://media.discordapp.net/attachments/724306793819275309/1013378822239944814/unknown.png">

## USB issues (need help and contributors):

Internal USB 3.0 doesnt work although USB 2.0 ones should work, I have no idea why but since switched to MacPro7,1, USB mapping actually does something so uploading USB map kexts in case it does something with your machine! ^^ 

But generally check this README anyways:
https://github.com/corpnewt/USBMap/blob/master/README.md#quick-start


For working USB 3.0 I used this PCIe expansion card (native support, no kexts needed), although keep figuring out why BIOS doesn't care about it, appears only on OS level:
https://www.amazon.pl/Inateck-Karta-USB-porty-ExpresCard/dp/B00HJ1DULE?th=1

## Monterey NOTE:

Current config is prepared for booting Ventura so if you want to run Monterey with supported dGPU, **revert Ventura NOTE steps.**

For unsupported dGPU, follow the steps below:

1. SET SIP to 0x802:

`NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> csr-active-config` to `02080000`

2. Disable Apple Secure Boot:

`Misc -> Security -> SecureBootModel` to `Disable` 

`Misc -> Security -> DmgLoading` to `Any`

3. Reset NVRAM using `ResetNvramEntry.efi` in `EFI\OC\DRIVERS`

3. (Optional) For auto root patching your unsupported dGPU - add `AutoPkgInstaller.kext` to your `EFI\OC\KEXTS`:

https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/payloads/Kexts/Acidanthera/AutoPkgInstaller-v1.0.0-DEBUG.zip

4. Follow OCLP prompts and reboot.

Done! GPU acceleration in Monterey is back again! 

Downside is need to every time gather InstallerAssistants.pkg from e.g. MrMacintosh's blog:
https://mrmacintosh.com/macos-12-monterey-full-installer-database-download-directly-from-apple/

(Thats if you choose to stay with MacPro7,1!)

## Ventura NOTE - OCLP status and the 'how to'...

Source: https://github.com/dortania/OpenCore-Legacy-Patcher/issues/998

For macOS 13 B3+ use OpenCore 0.8.3+

1. Natively supported dGPUs require AVX2 support so you are dependent only on Legacy Metal dGPU and OCLP!

https://github.com/dortania/OpenCore-Legacy-Patcher/issues/998#issuecomment-1166340370

2. Lack of AVX2 instruction set requires more fun with macOS 13 installation, so be aware! You need M1s dylds cache as its non AVX2 cache to boot successfully!

https://github.com/dortania/OpenCore-Legacy-Patcher/issues/998#issuecomment-1163607808

You can extract ipsw file from Apple Developer site or use link below:

https://www.theiphonewiki.com/wiki/Beta_Firmware/Mac/13.x

I've already applied mentioned patch for 'Root Hash verification' into config.plist but dylds with every update must be done manaully on your end.

3. OCLP now works with Ventura but its still in development and aside beta OS bugs you will gonna have also bugs related to your legacy Metal dGPU:

https://github.com/dortania/OpenCore-Legacy-Patcher/actions?query=branch%3Aventura-alpha

4. OCLP preparation (already applied, listing to actually teach you something):

1. SET SIP to 0xA03:

`NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> csr-active-config` to `03080000`

2. Disable Apple Secure Boot:

`Misc -> Security -> SecureBootModel` to `Disable` 

`Misc -> Security -> DmgLoading` to `Any`

3. Disable AMFI (+Fix for Electron apps on 12.3+):
Add `amfi_get_out_of_my_way=1 ipc_control_port_options=0` to `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`

4. Reset NVRAM using `ResetNvramEntry.efi` in `EFI\OC\DRIVERS`

5. (Optional) For auto root patching your unsupported dGPU generate `AutoPkgInstaller.kext` and add it to your `EFI\OC\KEXTS`:

https://github.com/dortania/OpenCore-Legacy-Patcher/blob/ventura-alpha/payloads/Kexts/Acidanthera/AutoPkgInstaller-v1.0.1-DEBUG.zip

6. Follow OCLP prompts and reboot.

Done! GPU acceleration in Ventura is working!

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
### OpenCanopy's resources:
https://github.com/acidanthera/OcBinaryData
### OpenCore Legacy Patcher:
https://github.com/dortania/OpenCore-Legacy-Patcher
