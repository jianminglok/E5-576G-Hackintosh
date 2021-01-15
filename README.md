
# Acer Aspire E 15 E5-576G Hackintosh

macOS Catalina/Big Sur on Acer E5-576G

## Configuration

| Specifications | Detail                                                  |
| ------------------- | ------------------------------------------- |
| Computer model      | Acer Aspire E 15 E5-576G (MX150)      |
| Processor           | Intel Core i5-8250U Processor     |
| Memory              | 8GB DDR3 1600MHz              |
| Integrated Graphics | Intel UHD Graphics 620                     |
| Sound Card          | Realtek ALC255 (layout-id:30)           |
| Wireless Card       | Intel Wireless 3165                        |


## Current Status in OpenCore

- Discrete graphic card is not working, since macOS doesn't support Optimus technology
  - Have used [SSDT-DGPU](EFI/OC/ACPI/SSDT-DGPU.dsl) to disable it in order to save power
- Intel Bluetooth may cause sleep problems and does not support some Bluetooth devices
  - View [Work-Around-with-Bluetooth](https://github.com/daliansky/XiaoMi-Pro/wiki/Work-Around-with-Bluetooth) (Untested)
- Intel Wi-Fi(Intel Wireless 3165) is working
  - Buy a USB Wi-Fi dongle or supported wireless card
  - Use [AirportItlwm](https://github.com/OpenIntelWireless/itlwm) to enable Intel Wi-Fi
- Using a HDMI-VGA adapter to connect an external monitor causes lag and black screen for unknown reason, but using either a HDMI-HDMI or VGA-VGA direct connection is fine.
- Everything else should work well



## Installation

### First-time installation

- Please refer to the following installation tutorials
  - [Xiaomi Mi Notebook Pro MacOS Catalina Installation Guide || ENGLISH](https://bit.ly/34biTqw)
- You can refer to the following tutorials for additional configuration
   - https://dortania.github.io/vanilla-laptop-guide/
- If you intend to dual boot Windows on the same drive, it is recommended that you install Windows **first**, and then resizing the EFI System Partition (ESP) too at least 200MB with any partition assistant you like (but avoid GParted). 
- If there's any partition to the left of the Windows partition which you would have to shrink to extend the ESP, move it to the **right** side of the Windows partition so you can successfully resize the ESP.
- It is highly recommended that you use [ProperTree](https://github.com/corpnewt/ProperTree) to make any changes to the config.plist in `\EFI\OC`, otherwise you may corrupt the file.
- Add `BOOT` and `OC` folders to your EFI partition.
- As our laptop is only boots from `\EFI\Microsoft\Boot\bootmgfw.efi` (or maybe not, I didn't bother to mess with the EFI boot map just yet), you will have to rename `\EFI\Microsoft\Boot\bootmgfw.efi` to `\EFI\Microsoft\Boot\bootmgfw-orig.efi` and copy `\EFI\refind\refind.efi` to \EFI\Microsoft\Boot and rename it as `\EFI\Microsoft\Boot\bootmgfw.efi` so you will see the refind boot picker every time.
- It is no longer required to rename `\EFI\Microsoft\Boot\bootmgfw.efi` to `\EFI\Microsoft\Boot\bootmgfw-orig.efi` from OC 0.5.8. Instead, we will use the new Bootstrap option in OC 0.5.8 to prevent Windows from replacing `\EFI\Microsoft\Boot\bootmgfw.efi`.
- For users with the E5-576G-59S6, please remove `\EFI\OC\ACPI\SSDT-TPDE.aml` and keep `\EFI\OC\ACPI\SSDT-TPD1.aml`. Also remove `\EFI\OC\config.plist` and rename `\EFI\OC\config_59S6.plist` to `EFI\OC\config.plist` due to issues with the touchpad. Other users of E5-576G will only need to keep `\EFI\OC\ACPI\SSDT-TPDE.aml` and `\EFI\OC\config.plist`, `\EFI\OC\ACPI\SSDT-TPD1.aml` and `\EFI\OC\config_59S6.plist` can be removed, unless a different touchpad patch needs to be made for your mode l too.

## FAQ

### My touchpad isn't working after update.

You need to rebuild the kext cache after every system update. Use `Kext Utility.app` or type `sudo kextcache -i /` in `Terminal.app`. Then restart. If this still doesn't work, try to press F9.

### My screen turns to black and has no response during the updating process.

If you have black screen for five minutes and get no response from the device, please force restart your laptop(Long press power button) and choose `Boot macOS Install from ~` entry.


## Credits

- Thanks to [Acidanthera](https://github.com/acidanthera) for providing [AppleALC](https://github.com/acidanthera/AppleALC), [AppleSupportPkg](https://github.com/acidanthera/AppleSupportPkg),  [Lilu](https://github.com/acidanthera/Lilu), [OcBinaryData](https://github.com/acidanthera/OcBinaryData), [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg), [VirtualSMC](https://github.com/acidanthera/VirtualSMC), [VoodooInput](https://github.com/acidanthera/VoodooInput), [VoodooPS2](https://github.com/acidanthera/VoodooPS2), and [WhateverGreen](https://github.com/acidanthera/WhateverGreen).
- Thanks to [alexandred](https://github.com/alexandred) for providing [VoodooI2C](https://github.com/alexandred/VoodooI2C).
- Thanks to [daliansky](https://github.com/daliansky) for providing [OC-little](https://github.com/daliansky/OC-little), the template for this Readme, many valuable samples in patching SSDT and various ACPI patches for OpenCore.
- Thanks to [jsassu20](https://github.com/jsassu20) for providing samples for [patching the brightness keys i ACPI](https://github.com/jsassu20/OpenCore-HotPatching-Guide/tree/master/17-Brightness%20Shortcut%20Patch).
- Thanks to [Sniki](https://github.com/Sniki) for providing [OS-X-USB-Inject-All](https://github.com/Sniki/OS-X-USB-Inject-All).
- Thanks to [corpnewt](https://github.com/corpnewt) for providing [USBMap](https://github.com/corpnewt/USBMap) and [ProperTree](https://github.com/corpnewt/ProperTree).
- Thanks to [zxystd](https://github.com/zxystd) for providing [IntelBluetoothFirmware](https://github.com/zxystd/IntelBluetoothFirmware).
