# Flex-2-15-Haswell-

Work in Progress

**Lenovo Flex 2-15**

- CPU:            i3-4030u
- GFX:            Intel HD 4400
- GFX/Optimus:    Nvidia GT 820M
- Display:        1366x768x32
- Lan:            RTL8111/8168/8411
- Wifi/BT:        RTL8723BE
- Sound:          Realtek ALC233

**Recommend BIOS Settings:**

- Boot Mode:		UEFI only
- Secure Boot:	Disabled
- xHCI Mode:		Smart Auto
- Graphics:			Discrete
- Bootorder:		Create entry with "Easy UEFI" pointing to /EFI/CLOVER/CLOVERX64.efi on your EFI partition

**Install Guide**

- http://www.tonymacx86.com/yosemite-desktop-guides/144426-how-install-os-x-yosemite-using-clover.html

**DSDT/SDDTs:**

- grep with Clover (fn+F4 and/or F4 at Clover boot screen) / files will be placed in /EFI/CLOVER/ACPI/origin
- remove everything expect DSDT.aml and SSDT*.aml, also remove SSDT-1.aml, SSDT-4x.aml, SSDT-5x.aml, SSDT-6x.aml, SSDT-7x.aml
- disassembly all at once after moving to another folder by "iasl -da -dl *.aml"

**Necessary DSDT Patches:**

1. DSDT
-

2. SSDT-2
-

2. SSDT-3
-

2. SSDT-8
-

2. SSDT-9
-

2. SSDT-10
-

**Necessary Kexts:**

- http://www.hwsensors.com/releases
- https://github.com/RehabMan/OS-X-Realtek-Network
- https://github.com/RehabMan/OS-X-Fake-PCI-ID
- https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller
- https://github.com/RehabMan/OS-X-Intel-Backlight
- https://github.com/the-darkvoid/EAPD-Codec-Commander
- http://www.insanelymac.com/forum/topic/298663-applehda-for-yosemite/ choose **AppleHDA-272.18.1-ALC233.zip**

Install to /EFI/CLOVER/kexts/10.10:

- FakeSMC.kext
- FakePCIID.kext
- FakePCIID_HD4600_HD4400.kext
- RealtekRTL8111.kext
- VoodooPS2Controller.kext

Install to /System/Library/Extensions:

- FakeSMC.kext
- ACPISensors.kext
- CPUSensors.kext
- FakePCIID.kext
- FakePCIID_HD4600_HD4400.kext
- RealtekRTL8111.kext
- VoodooPS2Controller.kext

- AppleHDA.kext
- CodecCommander.kext
- FakePCIID_XHCIMux.kext
- IntelBacklight.kext

**Plists to replace:**

- in FakeSMC.kext / added FANs and AC with 60W
- in VoodooPS2Keyboard.kext / located in VoodooPS2Controller.kext/Contents/PlugIns / edited keymap for Flex 2-15

**Kexts to patch:**

- AppleGraphicsPowerManagement.kext / put lowest GPUFrequency to 200Mhz and enable all powerstates
- AppleUSBCardReader.kext / to make internal SDCardReader Apple compatible

**Clover:**

- use included config.plist

**ICC-Profile:**
- put profile into ~/Library/ColorSync/Profiles and select for internal display
