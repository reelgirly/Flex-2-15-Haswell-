# Flex-2-15-Haswell-Yosemite-

Work in Progress

**Lenovo Flex 2-15**

- CPU:            i3-4030u
- GFX:            Intel HD 4400
- GFX/Optimus:    Nvidia GT 820M
- Display:        1366x768x32
- Lan:            RTL8111/8168/8411
- Wifi/BT:        RTL8723BE
- Sound:          Realtek ALC233
- Nvram:          Native

**Recommend BIOS Settings:**

- Boot Mode:		UEFI only
- Secure Boot:	Disabled
- xHCI Mode:		Smart Auto
- Graphics:			Discrete
- Bootorder:		Create entry with "Easy UEFI" pointing to /EFI/CLOVER/CLOVERX64.efi on your EFI partition

**Install Guide**

- http://www.tonymacx86.com/yosemite-desktop-guides/144426-how-install-os-x-yosemite-using-clover.html

To install OSX along to your OEM Windows install,
first shrink your windows C: partition with
http://www.disk-partition.com/free-partition-manager.html
and then create by your CloverUSB using DiskUtility a "HFS Extended Journaled" partition into the free space.
Not sure if OneKeyRecovery still working after this as i didnt test it.

Now install OSX into the newly created partition.

After installing OSX and booting into, install Clover into your already existing HDDs EFI partiton as UEFI only.

**Attention!** Rename the folder /EFI/Boot to /EFI/Boot_org to keep your Microsoft efi file before installing Clover.

**Choose OsxAptioFix2Drv-64.efi instead OsxAptioFixDrv-64.efi when installing clover to your HDDs EFI.**


**Clover:**

clover/config.plist
- Now copy the here attached config.plist to /EFI/CLOVER on your EFI partition


**Necessary Kexts:**

Then install these necessary kexts.

- http://www.hwsensors.com/releases
- https://github.com/RehabMan/OS-X-ACPI-Battery-Driver
- https://github.com/RehabMan/OS-X-Fake-PCI-ID
- https://github.com/RehabMan/OS-X-Intel-Backlight
- https://github.com/RehabMan/OS-X-Realtek-Network
- https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller
- https://github.com/the-darkvoid/EAPD-Codec-Commander
- http://www.insanelymac.com/forum/topic/298663-applehda-for-yosemite/ choose **AppleHDA-272.18.1-ALC233.zip**

Copy to /EFI/CLOVER/kexts/10.10:

- FakeSMC.kext
- FakePCIID.kext
- FakePCIID_HD4600_HD4400.kext
- RealtekRTL8111.kext
- VoodooPS2Controller.kext

Install to /System/Library/Extensions with "Kext Wizard":

- FakeSMC.kext
- ACPISensors.kext
- CPUSensors.kext
- FakePCIID.kext
- FakePCIID_HD4600_HD4400.kext
- RealtekRTL8111.kext
- VoodooPS2Controller.kext
- ACPIBatteryManager.kext
- AppleHDA.kext
- CodecCommander.kext
- FakePCIID_XHCIMux.kext
- IntelBacklight.kext

After being ready, repair permissions with DiskUtility and reboot into Clover.


**Must Read Guide**

Download latest RehabMan-MaciASL-*.zip and RehabMan-patchmatic-*.zip from:

https://bitbucket.org/RehabMan/os-x-maciasl-patchmatic/downloads

And latest iasl.zip (Unzip and copy "iasl" to "/usr/bin") from:

https://bitbucket.org/RehabMan/acpica/downloads

Now implement the Rehabman Repository into MaciASL.  Further Instructions are here:

https://github.com/RehabMan/Laptop-DSDT-Patch


**Must Read Guide**

Get into DSDT/SSDT patching after installing the necessary tools from above. Read the whole thing to understand what you will do next.

- http://www.tonymacx86.com/yosemite-laptop-support/152573-guide-patching-laptop-dsdt-ssdts.html


**DSDT/SDDTs:**

- Now reboot into Clover and grep the ACPI files (press Fn+F4 and/or F4 at Clover boot screen)
- files will be placed in /EFI/CLOVER/ACPI/origin
- move all files to another folder
- remove everything except DSDT.aml and the SSDT*.aml files
- remove SSDT-1.aml, SSDT-4x.aml
- disassembly all at once by "iasl -da -dl *.aml" in terminal where files are located
- then delete/move all AML files and just leave the DSL in this folder


**Necessary DSDT Patches:**

**After applying all patches to each DSL file check if it compiles without ERRORS and save it.**

**DSDT.dsl**
- [syn] Fix ADBG Error
- [syn] Remove _DSM methods
- [sys] IRQ Fix
- [sys] SMBUS Fix
- [sys] RTC Fix
- [sys] Shutdown Fix v2
- [sys] HPET Fix
- [sys] OS Check Fix (Windows 8)
- [sys] AC Adapter Fix
- [sys] Add MCHC
- [sys] Fix _WAK Arg0 v2
- [sys] Fix Mutex with non-zero SyncLevel
- [sys] Add IMEI
- [sys] Haswell LPC
- [usb] USB3 _PRW(0x6D) and Rename XHC to XHC1
- [usb] 7-series/8-series USB
- [audio] Audio Layout 3
- [igpu] Rename GFX0 to IGPU
- [igpu] Rename PCI0.GFX0 to PCI0.IGPU
- [igpu] Rename B0D3 to HDAU
- patches/dsdt_fan.txt
- patches/dsdt_keyboard.txt
- patches/dsdt_nvidia.txt

**SSDT-0.dsl**
- [syn] Remove _DSM methods

**SSDT-2.dsl**
- [syn] Remove _PSS placeholders

**SSDT-3.dsl**
- none right now

**SSDT-8.dsl**
- [syn] Remove _DSM methods
- [igpu] Haswell HD4400/HD4600/HD5000
- [igpu] Brightness fix (Haswell/Broadwell)
- [igpu] Rename GFX0 to IGPU
- [igpu] Rename PCI0.GFX0 to PCI0.IGPU
- [igpu] Rename B0D3 to HDAU
- patches/ssdt-8_graphics.txt
- patches/ssdt-8_hdmi.txt

**SSDT-9.dsl**
- patches/ssdt-9_nvidia.txt

**SSDT-10.dsl**
- [syn] Remove _DSM methods

If all files compiled without errors open terminal again in folder where the files are located and compile them all at once by "iasl *.dsl".

Now copy ur compiled **AML** files to /EFI/CLOVER/ACPI/patched on your EFI partition

You should have there:

- DSDT.aml
- SSDT-0.aml
- SSDT-2.aml
- SSDT-3.aml
- SSDT-8.aml
- SSDT-9.aml
- SSDT-10.aml


**Native Powermanagement:**

Follow this Guide to complete native power management.
- http://www.tonymacx86.com/yosemite-laptop-support/146870-guide-native-power-management-laptops.html


**Plists to replace:**

FakeSMC.kext/Info.list
- added FANs and AC with 60W

Copy here attached Info.plist into /EFI/CLOVER/kexts/10.10/FakeSMC.kext/Contents and /System/Library/Extensions/FakeSMC.kext/Contents.


VoodooPS2Keyboard.kext/Info.plist
- located in VoodooPS2Controller.kext/Contents/PlugIns
- edited keymap for Flex 2-15

Copy here attached Info.plist into /EFI/CLOVER/kexts/10.10/VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Keyboard.kext/Contents and /System/Library/Extensions/VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Keyboard.kext/Contents.



**Kexts to patch by hand:**

**Backup kexts before if anything goes wrong.**

Use Texteditor or Plisteditor (TextWrangler for example; allows editing Systemfiles by unlocking)

AppleGraphicsPowerManagement.kext/changes.txt
- put lowest GPUFrequency to 200Mhz and enable all powerstates

Find and Replace necessary parts in /System/Library/Extensions/AppleGraphicsPowerManagement.kext/Contents/Info.plist


AppleUSBCardReader.kext/changes.txt
- located in AppleStorageDrivers.kext/Contents/PlugIns
- make internal SDCardReader Apple compatible

Find and Replace necessary parts in /System/Library/Extensions/AppleStorageDrivers.kext/Contents/PlugIns/AppleUSBCardReader.kext/Contents/Info.plist


IOBluetoothFamily.kext/changes.txt
- keeps USB 2.0 working if you are not having an AirPort compatible wifi card
- keeps LogitechControlCenter working to use a Logitech USB Mouse

Find and Delete mentioned kexts from /System/Library/Extensions/IOBluetoothFamily.kext/Contents/PlugIns


**ICC-Profile:**

display/icc_profile.txt
- download from link and put profile into ~/Library/ColorSync/Profiles and apply for internal display
