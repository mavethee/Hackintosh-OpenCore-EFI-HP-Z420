<img src="https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Logos/OpenCore_with_text_Small.png" width="200" height="48"/>

## Hackintosh-OpenCore-HP-Z420
EFI premade of OpenCore bootloader for HP-Z420 is here!

## Current version - OpenCore 0.8.1 DEBUG
Repository contains full ,,Plug-and-Play" EFI of OpenCore bootloader and
all needed files to install and run macOS on HP Z420!

https://github.com/acidanthera/OpenCorePkg/releases/tag/0.8.0

I've used natively supported GPU, you should too ;)

Internal USBs doesnt work so USBInjectAll was skipped!

For working USBs I used this PCIe expansion card (native support, no kexts needed):
https://www.amazon.pl/Inateck-Karta-USB-porty-ExpresCard/dp/B00HJ1DULE?th=1

OCLP DOESN'T WORK FOR MACOS 13, USE MACPRO7,1 SMBIOS AND NATIVELY SUPPORTED DGPU!
Memory mismatch error fix: https://dortania.github.io/OpenCore-Post-Install/universal/memory.html
Or just use RestrictEvents included in Kexts.

There are some issues in macOS 13 related to lack of AVX2 instruction set, requires more fun with macOS 13 beta 1 installer, at own risk lol.

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
 
