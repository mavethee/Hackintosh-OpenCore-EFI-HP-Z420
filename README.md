<img src="https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Logos/OpenCore_with_text_Small.png" width="200" height="48"/>

## Hackintosh-OpenCore-HP-Z420
EFI premade of OpenCore bootloader for HP-Z420 is here!

## Current version - OpenCore 0.7.7 (11th January 2022!)
Repository contains full ,,Plug-and-Play" EFI of OpenCore bootloader and
all needed files to install and run macOS on DELL Inspiron 15 7000 Gaming (7567)!

https://github.com/acidanthera/OpenCorePkg/releases/tag/0.7.7

### Please note:
Since OC 0.6.5, I decided to switch to RELEASE version, if you expierience any issues, switch to debug using Dortania's Guide:

https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/debug.html

I've used natively supported GPU, you should too ;)

Internal USBs doesnt work so USBInjectAll was skipped!

For working USBs I used this PCIe expansion card (native support, no kexts needed):
https://www.amazon.pl/Inateck-Karta-USB-porty-ExpresCard/dp/B00HJ1DULE?th=1

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
 
