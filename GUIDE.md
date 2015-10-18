# Flex 2-15 Haswell -Yosemite-

# Work in Progress

## **Lenovo Flex 2-15**

- CPU:            i3-4030u
- GFX:            Intel HD 4400
- GFX/Optimus:    Nvidia GT 820M
- Display:        1366x768x32
- Lan:            RTL8111/8168/8411
- Wifi/BT:        RTL8723BE
- Sound:          Realtek ALC233
- BIOS:           ver. A0CN37WW
- Nvram:          Native

### **Recommend BIOS Settings:**

- Boot Mode:		UEFI only
- Secure Boot:	Disabled
- xHCI Mode:		Smart Auto
- Graphics:			Discrete
- Sata:         AHCI
- Bootorder:		Create EFI entry with "Easy UEFI" or Linux pointing to /EFI/CLOVER/CLOVERX64.efi on your EFI partition

### **Install Guide**

[Booting the OS X installer on LAPTOPS with Clover](http://www.tonymacx86.com/yosemite-laptop-support/148093-guide-booting-os-x-installer-laptops-clover.html)

> **Attention!** make full disk backup before trying to install OSX next to your existing Windows install !!!

> To install OSX along to your OEM Windows install,
first shrink your windows C: partition with
[AMOEI](http://www.disk-partition.com/free-partition-manager.html)
and then create by your CloverUSB using DiskUtility a "HFS Extended Journaled" partition into the free space.
Not sure if OneKeyRecovery still working after this as i didnt test it.

> Now install OSX into the newly created partition.

> After installing OSX and booting into, install Clover into your already existing HDDs EFI partiton as "UEFI only".

> **Attention!** Rename the folder /EFI/Boot to /EFI/Boot_org to keep your Microsoft efi file before installing Clover.

**Choose OsxAptioFix2Drv-64.efi instead OsxAptioFixDrv-64.efi when installing clover to your HDDs EFI.**


### **Clover:**

Now copy this **[config.plist](clover/config.plist)** to /EFI/CLOVER on your EFI partition


### **Necessary Kexts:**

Then install these necessary kexts.

- http://www.hwsensors.com/releases
- https://github.com/RehabMan/OS-X-ACPI-Battery-Driver
- https://github.com/RehabMan/OS-X-Fake-PCI-ID
- https://github.com/RehabMan/OS-X-Intel-Backlight
- https://github.com/RehabMan/OS-X-Realtek-Network
- https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller
- https://github.com/the-darkvoid/EAPD-Codec-Commander
- http://www.insanelymac.com/forum/topic/298663-applehda-for-yosemite/ choose *AppleHDA-272.18.1-ALC233.zip*

**Copy to /EFI/CLOVER/kexts/10.10:**

> - FakeSMC.kext
- FakePCIID.kext
- FakePCIID_HD4600_HD4400.kext
- RealtekRTL8111.kext
- VoodooPS2Controller.kext

**Install to /System/Library/Extensions with "Kext Wizard":**

> - FakeSMC.kext
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


### **Necessary Tools**

Download latest *RehabMan-MaciASL-.zip* and *RehabMan-patchmatic-.zip* from:

https://bitbucket.org/RehabMan/os-x-maciasl-patchmatic/downloads

And latest *iasl.zip* (Unzip and copy "iasl" to "/usr/bin") from:

https://bitbucket.org/RehabMan/acpica/downloads

Now implement the Rehabman Repository into MaciASL.  Further Instructions are here:

https://github.com/RehabMan/Laptop-DSDT-Patch


### **Must Read Guide**

[Patching LAPTOP DSDT/SSDTs](http://www.tonymacx86.com/yosemite-laptop-support/152573-guide-patching-laptop-dsdt-ssdts.html)

>Get into DSDT/SSDT patching after installing the necessary tools from above.

>Read the whole thing to understand what you will do next.

**After being ready, repair permissions with DiskUtility and reboot into Clover.**


### **DSDT/SDDTs:**

- Now in Clover grep the ACPI files (press Fn+F4 and/or F4 at Clover boot screen)
- files will be placed in /EFI/CLOVER/ACPI/origin
- boot into your OSX
- move all files to another folder
- remove everything except DSDT.aml and the SSDT*.aml files
- remove SSDT-1.aml, SSDT-4x.aml
- disassembly all at once by "iasl -da -dl *.aml" in terminal where files are located
- then delete/move all AML files and just leave the DSL in this folder


### **Necessary DSDT Patches:**

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
- [audio] Audio Layout 3
- [igpu] Rename GFX0 to IGPU
- [igpu] Rename PCI0.GFX0 to PCI0.IGPU
- [igpu] Rename B0D3 to HDAU
- [dsdt_usb](patches/dsdt_usb.txt)
- [dsdt_fan](patches/dsdt_fan.txt)
- [dsdt_keyboard](patches/dsdt_keyboard.txt)
- [dsdt_nvidia](patches/dsdt_nvidia.txt)

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
- [ssdt-8_graphics](patches/ssdt-8_graphics.txt)
- [ssdt-8_hdmi](patches/ssdt-8_hdmi.txt)

**SSDT-9.dsl**
- [ssdt-9_nvidia](patches/ssdt-9_nvidia.txt)

**SSDT-10.dsl**
- [syn] Remove _DSM methods

**If all files compiled without errors:**

> 1. Open terminal pointing to the folder where all files located and compile them all at once by using "iasl *.dsl"
>   - And copy only your **patched AML** files to /EFI/CLOVER/ACPI/patched on your EFI partition.

> 2. Or save them OneByOne with MaciASL as **AML** to /EFI/CLOVER/ACPI/patched

**You should now have at /EFI/CLOVER/ACPI/patched:**

> - DSDT.aml
- SSDT-0.aml
- SSDT-2.aml
- SSDT-3.aml
- SSDT-8.aml
- SSDT-9.aml
- SSDT-10.aml


### **Native Powermanagement:**

[Native Power Management for Laptops](http://www.tonymacx86.com/yosemite-laptop-support/146870-guide-native-power-management-laptops.html)

Follow this Guide to complete native power management.
Place generated SSDT.aml in /EFI/CLOVER/ACPI/patched.

**You should now have at /EFI/CLOVER/ACPI/patched:**

> - DSDT.aml
- SSDT.aml
- SSDT-0.aml
- SSDT-2.aml
- SSDT-3.aml
- SSDT-8.aml
- SSDT-9.aml
- SSDT-10.aml


### **Plists to replace:**

**FakeSMC.kext**
- added FAN and AC with 60W

> Copy attached [Info.plist](FakeSMC.kext/Info.list) into /EFI/CLOVER/kexts/10.10/FakeSMC.kext/Contents and /System/Library/Extensions/FakeSMC.kext/Contents


**VoodooPS2Keyboard.kext**
- located in VoodooPS2Controller.kext/Contents/PlugIns
- edited keymap for Flex 2-15

> Copy attached [Info.plist](VoodooPS2Keyboard.kext/Info.plist) into /EFI/CLOVER/kexts/10.10/VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Keyboard.kext/Contents and /System/Library/Extensions/VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Keyboard.kext/Contents


### **Kexts to patch by hand:**

**Backup kexts before if anything goes wrong or you will later add a compatible Wifi/BT card.**

Use Texteditor or Plisteditor (TextWrangler for example; allows editing of Systemfiles by unlocking)

**[AppleGraphicsPowerManagement.kext](AppleGraphicsPowerManagement.kext/changes.txt)**
- put lowest GPUFrequency to 200Mhz and enable all powerstates

> Find and Replace necessary parts in /System/Library/Extensions/AppleGraphicsPowerManagement.kext/Contents/Info.plist


**[AppleUSBCardReader.kext](AppleUSBCardReader.kext/changes.txt)**
- located in AppleStorageDrivers.kext/Contents/PlugIns
- make internal SDCardReader Apple compatible

> Find and Replace necessary parts in /System/Library/Extensions/AppleStorageDrivers.kext/Contents/PlugIns/AppleUSBCardReader.kext/Contents/Info.plist


**[IOBluetoothFamily.kext](IOBluetoothFamily.kext/changes.txt)**
- keeps USB 2.0 working if you are not having an AirPort compatible wifi card
- keeps LogitechControlCenter working to use a Logitech USB Mouse

> Find and Delete mentioned kexts from /System/Library/Extensions/IOBluetoothFamily.kext/Contents/PlugIns


### **ICC-Profile:**

Download this [ICC-Profile](http://www.notebookcheck.com/uploads/tx_nbc2/Monitor_8-5-2014_1.icc) and put profile into ~/Library/ColorSync/Profiles and apply for internal display.

**After being ready, repair permissions with DiskUtility and rebuild KernelCaches with "Kext Wizard" and reboot twice.**
