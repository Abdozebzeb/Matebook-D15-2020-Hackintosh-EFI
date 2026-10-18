 Huawei MateBook D15 2020 (BOHB-WAX9) - OpenCore Hackintosh

This repository provides an OpenCore EFI configuration and setup instructions for running macOS on the **Huawei MateBook D15 2020 (BOHB-WAX9)**.

> **Compatibility Note:** This EFI build is fully optimized and tested for **macOS Tahoe 26.5.1**. Please note that installing macOS on non-Apple hardware may require extra troubleshooting and could have minor feature limitations depending on hardware revisions.

---

![Huawei Matebook D15 2020 running macOS](./assets/tahoe_image_laptop.png.jpg)

---




## Hardware Specifications

| Component | Specification | Status |
| --- | --- | --- |
| **Model** | Huawei MateBook D15 2020 (`BOHB-WAX9`) | Fully Supported |
| **CPU** | Intel Core i3-10110U @ 2.10GHz (Comet Lake) | Working |
| **GPU** | Intel UHD Graphics | Working (Full Acceleration) |
| **RAM** | 8GB DDR4 | Working |
| **Storage** | 256GB NVMe SSD | Working |
| **Wi-Fi / BT** | Intel Wireless-AC 9560 (160MHz) | Working (`itlwm` / `AirportItlwm`) |
| **Audio** | Everest ES8316 | **Unsupported** (Workaround required) |

---

## Known Issues & Limitations

* **Audio:** The Everest ES8316 codec currently lacks native macOS driver support. Internal speakers and the 3.5mm headphone jack will not output sound.
* **Workaround:** Use a USB audio adapter, Type-C DAC dongle, or Bluetooth headphones.



---

## Prerequisites

Before starting, ensure you have:

* A **16GB+ USB flash drive** for the installer.
* A macOS Tahoe 26.5.1 installer image. (Follow the [Dortania Installation Guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/#making-the-installer)).
* The latest release of the [OpenCore Bootloader](https://github.com/acidanthera/OpenCorePkg).
* [OpenCore Auxiliary Tools (OCAT)](https://www.google.com/search?q=https://github.com/icooky/OCAT) for editing `config.plist`.
* [Hasleo EasyUEFI](https://www.easyuefi.com/index-cn.html) or [AOMEI Partition Assistant / Partition Wizard] for dual-boot partition management on Windows.

---

## Installation Guide

### Step 1: Prepare the USB Installer

1. Create a bootable macOS installer on your USB drive using Dortania’s guide.
2. Clone or download this repository.
3. Copy the provided `EFI` directory directly to the root of your USB drive's EFI partition.

### Step 2: macOS Installation

1. Boot from the USB drive via the BIOS/UEFI boot selector.
2. Complete the macOS installation process. If you run into boot hangs or kernel panics, consult the [Dortania Troubleshooting Guide](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/troubleshooting.html).

---

## Post-Installation Setup

### Booting Without USB (Dual-Boot / Standalone Setup)

To boot macOS directly from your internal storage alongside Windows:

1. **Resize Partition (Windows):**
* Shrink `C:` drive to create **500 MB** of unallocated space.
* Format the 500 MB partition as **FAT32**.


2. **Reconfigure EFI Partition:**
* Delete the original 200 MB EFI partition using a partition tool (e.g., AOMEI / Partition Wizard).
* Generate standard Windows EFI files in the new 500 MB partition if needed.


3. **Mount the EFI Partition:**
* Open **Command Prompt** as Administrator and run:
```cmd
diskpart
list disk
select disk <disknumber>
list vol
select vol <efi_partition_number>
assign letter=Z
exit

```




4. **Add OpenCore Files:**
* Open **TreeSize** as Administrator (or right-click `Z:` drive and open via TreeSize) to browse the EFI partition.
* Copy the `OC` folder from this repository into `Z:\EFI\`, alongside the existing `Microsoft` and `Boot` folders.
* Rename `Z:\EFI\Boot\bootx64.efi` to `boot_fallback.efi`.
* Copy `bootx64.efi` from this repository's `EFI/Boot` directory into `Z:\EFI\Boot\`.


5. **Configure UEFI Boot Option:**
* Open **EasyUEFI**.
* Select **Manage EFI Boot Option** → **New** → **Linux or Other OS**.
* Name: `Hackintosh`
* Target Partition: Select the 500 MB EFI Partition.
* File Path: `\EFI\Boot\bootx64.efi`
* Save and move the `Hackintosh` entry to the top of the boot order.


6. **Validate Configuration:**
* Open `config.plist` on the EFI partition using **OCAT** and save it to resolve schema validation issues.



> ⚠️ **Important:** Do **NOT** reset/clear NVRAM after completing these steps. Clearing NVRAM will remove the custom boot entry created in EasyUEFI and may break Windows PIN/Hello sign-in options.

---

## Post-Install Checklist

* [Enabling iServices (iMessage, FaceTime, iCloud)](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html)
* [General OpenCore Post-Install Guide](https://dortania.github.io/OpenCore-Post-Install/)

---

## Contributing

Pull requests and issues are welcome! If you find a fix for the audio codec or performance improvements, feel free to submit a PR.

---

## Credits

* **EFI Maintainer:** Abdullah Zeb ([@abdozebzeb](https://github.com/Abdozebzeb))
* **Initial EFI Build:** [@goodbyepavlyi](https://github.com/goodbyepavlyi)
* [Acidanthera](https://github.com/acidanthera) for OpenCore Bootloader and drivers.
* [Dortania](https://dortania.github.io/) for Hackintosh documentation.


Note: I may Record a Video Showing this entire process from Scratch later.