# Flex-2-15-Haswell-

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
- Secure Boot:		Disabled
- xHCI Mode:		Smart Auto
- Graphics:			Discrete
- Bootorder:		Create entry with "Easy UEFI" pointing to /EFI/CLOVER/CLOVERX64.efi on your EFI partition

**Plists to replace:**

- in FakeSMC.kext / FAN and AC keys and 60Watts AC description
- in VoodooPS2Keyboard.kext / located in VoodooPS2Controller.kext/Contents/PlugIns / edited keymap for Flex 2-15

**Kexts to patch:**

- AppleGraphicsPowerManagement.kext / put lowest GPU-freq to 200Mhz aand enable all powerstates
- AppleUSBCardReader.kext / to make SDcardReader AppleCompatible

**Clover**

- use included config.plist

**ICC-Profile:**
- put profile into ~/Library/ColorSync/Profiles and select for internal display