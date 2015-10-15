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

To install OSX along to your OEM Windows install,
first shrink your windows C: partition with
http://www.disk-partition.com/free-partition-manager.html
and then install OSX from your just created CloverUSB to free space.
Not sure if OneKeyRecovery still working after this as i didnt test it.

**Must Read Guide**

- http://www.tonymacx86.com/yosemite-laptop-support/152573-guide-patching-laptop-dsdt-ssdts.html

**Necessary Kexts:**

- http://www.hwsensors.com/releases
- https://github.com/RehabMan/OS-X-ACPI-Battery-Driver
- https://github.com/RehabMan/OS-X-Fake-PCI-ID
- https://github.com/RehabMan/OS-X-Intel-Backlight
- https://github.com/RehabMan/OS-X-Realtek-Network
- https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller
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
- ACPIBatteryManager.kext
- AppleHDA.kext
- CodecCommander.kext
- FakePCIID_XHCIMux.kext
- IntelBacklight.kext

**DSDT/SDDTs:**

- grep with Clover (fn+F4 and/or F4 at Clover boot screen) / files will be placed in /EFI/CLOVER/ACPI/origin
- remove everything expect DSDT.aml and SSDT*.aml
- remove SSDT-1.aml, SSDT-4x.aml
- disassembly all at once after moving to another folder by "iasl -da -dl *.aml"
- then delete/move all AML files and just leave the DSL in this folder

**Necessary DSDT Patches:**

**After applying all patches to each DSL file check if it compiles without ERRORS**

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

If all files compiled without errors, dont forget to save and open terminal again in folder where the files are located and compile them all at once by "iasl *.dsl".

Now copy ur compiled **AML** files to /EFI/CLOVER/ACPI/patched on your EFI partition

You should have there:

- DSDT.aml
- SSDT-0.aml
- SSDT-2.aml
- SSDT-3.aml
- SSDT-8.aml
- SSDT-9.aml
- SSDT-10.aml

**Clover:**

clover/config.plist
- copy config.plist to /EFI/CLOVER on your EFI partition

**Native Powermanagement:**

Follow this Guide to make power management complete after booting with attached Clover config.plist
- http://www.tonymacx86.com/yosemite-laptop-support/146870-guide-native-power-management-laptops.html

**Plists to replace:**

FakeSMC.kext/Info.list
- in FakeSMC.kext / added FANs and AC with 60W

VoodooPS2Keyboard.kext
- located in VoodooPS2Controller.kext/Contents/PlugIns
- edited keymap for Flex 2-15

**Kexts to patch by hand:**

AppleGraphicsPowerManagement.kext/changes.txt
- put lowest GPUFrequency to 200Mhz and enable all powerstates

AppleUSBCardReader.kext/changes.txt
- located in AppleStorageDrivers.kext/Contents/PlugIns
- make internal SDCardReader Apple compatible

IOBluetoothFamily.kext/changes.txt
- keeps USB 2.0 working if you are not having an AirPort compatible wifi card
- keeps LogitechControlCenter working to use a Logitech USB Mouse

**ICC-Profile:**

display/icc_profile.txt
- download from link and put profile into ~/Library/ColorSync/Profiles and apply for internal display
