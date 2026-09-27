# Audion Setup Tools by Max.mov

**Contents**

- [Quick Start](#quick-start)
- [The Window](#the-window)
- [Card Buttons](#card-buttons)
- [Administrator Rights And Links](#administrator-rights-and-links)
- [Where The Program Keeps Its Own](#where-the-program-keeps-its-own)
- [Card Kinds](#card-kinds)
- [Project Layout](#project-layout)
- [Building](#building)
- [Complete Section And Function Map](#complete-section-and-function-map)
- [Operational Safety Reference](#operational-safety-reference)
- [Recommended Order](#recommended-order)
- [Driver And Update Review](#driver-and-update-review)
- [Disk And Storage Review](#disk-and-storage-review)
- [Display, GPU, And Cooling](#display-gpu-and-cooling)
- [FPS And Latency](#fps-and-latency)
- [Network, Browser, And Privacy](#network-browser-and-privacy)
- [Recovery After A Problem](#recovery-after-a-problem)
- [Completion Checklist](#completion-checklist)
- [Interface Map Maintenance](#interface-map-maintenance)

A program that sets Windows 10 and 11 up after a clean install. It is an interactive companion-remix of [Max.mov's guide](https://www.youtube.com/watch?v=ITdecD6R0Yw), not a line-by-line visualizer. Thirteen sections of action cards are built from `Manifests\Section##.psd1`, and one engine, `Engine\TweakEngine.psm1`, does the work. The window is written in Avalonia; PowerShell 7 is built into the program.

Microsoft recommendations and built-in Windows mechanisms have priority. `official` cards use documented sources from Microsoft, the hardware vendor, or the product author; `Max.mov guide` marks steps that definitely appear in Max.mov's guide; `Max.mov approved` is only for a recommendation that was separately and explicitly approved; `unofficial` keeps alternative and experimental solutions without presenting them as the official path.

## Quick Start

1. Double-click `Start.exe` in the root of the folder. Windows asks for administrator rights: the program changes system settings.
2. Pick a section on the left; its cards appear on the right. At start the program checks the state of every card for a few seconds; the result shows as a badge in the top right corner of a card.
3. A card's button does the step. The journal at the bottom shows how it goes.

It needs 64-bit Windows 10 or 11. Everything else is inside: there is no PowerShell 7 or .NET to install. If Windows refuses to run a downloaded file, open its properties and press Unblock.

## The Window

**Title strip.** The program's name, the RU/EN switch and the theme switch (sun - light, moon - dark). Then the screens, as Win+P: first only, second only, duplicate, extend (in 5 seconds), and the restart (in 15 seconds); while a countdown runs, the strip shows it with NOW and CANCEL. At the right, the machine: Windows and its build, the processor, the video card, the amount and type of memory, the system drive and its free space.

**Left panel.** Search over the cards, the list of sections and, at the bottom, Max.mov's YouTube and Telegram. Every section has its number, a colour rule of its theme and its number of cards. The panel is dark in both themes.

**Search.** Two letters or more show every matching card of every section, each group under its path "section › subsection". It searches the title, the description, the instruction and the id, in both languages at once. An empty field brings the chosen section back.

**A section.** Under its title are chips of its subsections: a click puts that subsection at the top of the view. Every subsection starts with a band with a coloured edge, its name and its number of cards; OPTIONAL, MAX.MOV and IN ORDER say it is optional, comes from the guide, or wants its steps top to bottom. With two switches or more the band has APPLY ALL and REVERT ALL: they go through the switches only, one by one, under one cancel. Links, installs and manual steps are never run as a batch.

**A card.**
- The bar at the left says where the step comes from or what it is about: neon purple - Max.mov's guide; green - NVIDIA, red - AMD, blue - Intel; light blue - the official route of Microsoft or the vendor; burnt orange - a side road.
- The icon is a brand mark (from Simple Icons) or an icon by meaning (from Lucide): location, Wi-Fi, a drive, a monitor and so on.
- Marks: ADMIN - administrator rights; RESTART - a restart is needed after the step; ESSENTIAL (green card border) - a step not to skip; CAREFUL (orange border) - a step that touches booting, drives, drivers or something hard to undo.
- The state at the top right: installed or applied; not installed or not applied; not for this machine - the card does not fit your hardware or build; unknown - the check did not work.

**Journal.** It opens with the first action: line by line it shows what the step does, and ends with "done in N s" or "did not work" and why. Buttons: CANCEL (stops the step, an installer too), COPY, FULL SCREEN / RESTORE, HIDE. JOURNAL at the bottom right opens and closes it at any time.

**Status line** - the version of the built-in PowerShell, the numbers of sections and cards, how long the state check took, the outcome of the last action.

## Card Buttons

| Button | What it does |
|---|---|
| APPLY / INSTALL / RUN | The main action: apply a setting, install a program with winget, run a script |
| OPEN IN WINDOWS | A page of Settings, Explorer or a Windows console |
| VERSIONS | On NVIDIA drivers and everything taken from TechPowerUp: the list of versions the source has opens under the card's buttons - date, size, installer or portable. The usual choice is ticked already: on NVIDIA the newest golden version (★ - stable LTS versions by users' ratings, `NvidiaGolden` in `Manifests\Downloads.psd1`, as in Audion Get Tools) for this card's series, on TechPowerUp the newest release. Tick what you need and press DOWNLOAD: the list closes, the download runs in the journal, TechPowerUp files are checked by SHA256. Betas are not listed: whoever wants a beta takes it from the site. On the Intel chipset card the package with the INF of this PC's Intel devices is ticked (the FOR THIS PC mark); every Intel installer is checked for Intel Corporation's signature. Intel ME and RST, Realtek network, Wi-Fi, Bluetooth and audio, MediaTek and Qualcomm Wi-Fi and Bluetooth come from the Microsoft Update Catalog by this PC's device id; only a version newer than the installed one is ticked: WHQL drivers without Intel's programs, the installed version marked (INSTALLED), unpacked into a folder of INF files, the journal gives the install command |
| INSTALLER | Downloads the latest release of the program into `Downloads\Audion Setup Tools\<program>` and shows it in Explorer |
| PORTABLE | Downloads the portable build and unpacks it into `Downloads\Audion Setup Tools\<program>\Portable` - nothing to install |
| INSTALL (on programs to download) | Downloads the installer and installs the program without questions |
| OPEN SITE / OPEN / SITE | The site in your default browser |
| Button colours | Bottle green - install, installers and versions; dark orange - portable builds; sea blue - a site or a page of Windows; mint - apply; burnt orange - revert or remove |
| REVERT / REMOVE | Put the previous value back, uninstall. For switches it appears once the program itself changed the value: the previous one is kept in `Data\backups` |
| WINDOWS DEFAULT | Put back the value Windows has out of the box. Works even when the setting was changed long before this program. Shown when the value is not the default now |

One action runs at a time. After each the program checks the card's state again.

## Administrator Rights And Links

The program runs as administrator, otherwise it could not change system settings. Pages, sites and folders still open as your ordinary self: the browser comes with your extensions and your accounts.

## Where The Program Keeps Its Own

Everything stays in the program's folder, in `Data\`: nothing goes into the user profile.
- `Data\backups\` - previous values of the settings the program changed. Not a cache: without these files REVERT has nothing to put back. Keep them while you want to be able to undo.
- `Data\lang.txt`, `Data\theme.txt` - the chosen language and theme. Russian and dark by default.

## Card Kinds

- `link` - opens a site in the browser.
- `deeplink` - opens a Windows page: `ms-settings:`, `shell:`, a console or the Microsoft Store.
- `docs` - opens the documentation folder.
- `manual` - a manual step: the program changes nothing, the instruction says what to do.
- `registry` - writes a registry value and keeps the previous one in a backup.
- `script` - a PowerShell script from the manifest: apply, revert, check the state.
- `service` - changes the startup type and state of a Windows service.
- `feature` - turns a Windows feature on or off.
- `powerscheme` - imports and activates a power scheme.

## Project Layout

```text
Audion Setup Tools by Max.mov/
├── Start.exe                     # start: asks UAC and opens the program
├── Bin/                          # the program AudionSetupTools.exe with PowerShell 7 and .NET inside
├── Engine/
│   ├── TweakEngine.psm1          # the engine: apply, revert, check
│   ├── Invoke-TweakProfile.ps1   # profile runner without a window, for AI agents
│   ├── Build-App.ps1             # builds the program into Bin\
│   └── Launcher/                 # source and build of Start.exe
├── Manifests/Section##.psd1      # sections and cards
├── Strings/ru.psd1, en.psd1      # the engine's messages in two languages
├── Assets/                       # icon, power schemes, scripts, Max.mov materials
├── Skill/                        # the AI-agent contract and curated profiles
├── Source/                       # the window's source (C#, Avalonia); Tools/ - icon table generators
├── Docs/                         # README and this guide; tools/ - map generators from the manifests
├── Tests/                        # Run-Checks.ps1 and EngineCheck: checks before a build
├── Data/                         # yours: backups, language, theme
├── builder_main.cmd              # builder
├── cleanup_project.cmd           # cleanup
└── config/version.json           # version
```

## Building

`builder_main.cmd` is a menu of steps; the orchestrator reads it by itself.

| Step | What it does |
|---|---|
| 01 BUILD APP | Checks, then publishes the program into `Bin\` |
| 02 START LAUNCHER | Builds `Start.exe` with the program icon |
| 03 CHECKS | Checks only: manifests load in PowerShell 5.1 and 7, the state of every card equals the built-in engine, `Start.exe` answers |
| 04 ICONS | Rebuilds the icon tables from Simple Icons and Lucide |
| 70 CLEAN CACHE | Removes build intermediates; `Bin\` stays |
| 71 VERIFY | Is everything in place, and which version |
| 95 OPEN Bin | Opens the program folder |

`cleanup_project.cmd` removes logs, scratch files and build intermediates. It clears `Data\backups` too - `/KEEPBACKUPS` leaves it.

## Complete Section And Function Map

<!-- card-map:start -->
Built from the manifests by `Docs\tools\Build-GuideMap.ps1` - not edited by hand. Sections 14, subsections 71, cards 333. Kinds: deeplink=59, docs=1, feature=1, link=137, manual=74, powerscheme=9, registry=17, script=34, service=1.

### 0. Windows Installation

#### 0.0 Data & Passwords

- **Project documentation** (`program-guide-readme-pdf` · documentation)
  - Description: Opens the complete documentation folder: user guides, technical README files, video companion notes, and other maintained project materials.
  - What it does: Opens the `Docs` folder with the documentation.
  - Buttons: OPEN
  - Note: Opens the Docs folder without choosing a specific file for the user.
- **Rename disks to their drive letters** (`rename-drives` · Windows page · administrator · from Max.mov's guide)
  - Description: Open Disk Management to verify and rename your partitions before reinstalling. Helps identify disks correctly after the fresh install.
  - What it does: Opens in Windows: `diskmgmt.msc`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Open Disk Management. Right-click each volume and rename it to match its drive letter (e.g. "C", "D"). This makes it easier to identify partitions during installation.
- **Back up your data** (`check-data-backup` · Windows page · **ESSENTIAL** · from Max.mov's guide)
  - Description: Open File Explorer and verify that all important files are backed up to an external drive or cloud storage before reinstalling Windows.
  - What it does: Opens in Windows: `shell:ThisPCFolder`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Check all drives for documents, downloads, desktop files, and any other data you want to keep. Copy them to an external drive or cloud storage.
- **Export browser passwords** (`check-browser-passwords` · manual step · **ESSENTIAL** · from Max.mov's guide)
  - Description: Export your saved passwords from Chrome before reinstalling. Navigate to chrome://settings/passwords and use the export option.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Open Chrome and go to chrome://settings/passwords. Click the three-dot menu next to "Saved passwords" and choose "Export passwords". Save the file to a backup location.
- **Verify account access (Microsoft, Steam, etc.)** (`check-account-access` · manual step · **ESSENTIAL** · from Max.mov's guide)
  - Description: Make sure you can log in to all important accounts — Microsoft, Steam, Epic Games, etc. — before reinstalling. Note down passwords or recovery options.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Test logins for: Microsoft/Xbox account, Steam, Epic Games, EA App, and any other services you use. If 2FA is involved, confirm your authenticator app works. Write down recovery codes if needed.

#### 0.1 GPU Driver Preparation

A subsection of Max.mov's guide.

- **Laptop note: download drivers for BOTH GPU and iGPU** (`laptop-igpu-note` · manual step)
  - Description: On most laptops the display is wired to the integrated GPU (iGPU), not the discrete GPU. Do NOT disable the iGPU. Download drivers for both the integrated and discrete graphics adapters.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: On a laptop with dual GPUs (e.g. Intel UHD + RTX 3050): download the driver for Intel UHD (iGPU) AND the Nvidia/AMD driver for the discrete GPU. Never disable the iGPU on a laptop — the built-in display is connected through it.
- **Find your GPU model** (`find-gpu-model` · Windows page)
  - Description: Open Device Manager to check the exact model of your graphics card before downloading the driver.
  - What it does: Opens in Windows: `devmgmt.msc`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Expand "Display adapters" in Device Manager to find your GPU model name.
- **Download Nvidia GPU driver** (`download-nvidia-driver-prep` · link · from Max.mov's guide)
  - Description: Official Nvidia driver download page. Download the latest Game Ready Driver for your GPU model.
  - What it does: Opens the site in your browser: `https://www.nvidia.com/en-us/drivers/`
  - Buttons: VERSIONS, SITE
- **Download AMD GPU driver** (`download-amd-driver-prep` · link · from Max.mov's guide)
  - Description: Official AMD driver download page. Download the latest Adrenalin driver for your GPU.
  - What it does: Opens the site in your browser: `https://www.amd.com/en/support/download/drivers.html`
  - Buttons: VERSIONS, SITE
- **Download Intel GPU / Arc driver (site availability varies by region)** (`download-intel-gpu-driver-prep` · link)
  - Description: Official Intel driver download center. Download the Intel graphics driver for your iGPU or Arc GPU.
  - What it does: Opens the site in your browser: `https://www.intel.com/content/www/us/en/download-center/home.html`
  - Buttons: VERSIONS, SITE
- **Move driver installers to a USB drive or separate partition** (`move-drivers-to-usb` · manual step · **ESSENTIAL** · from Max.mov's guide)
  - Description: After downloading GPU drivers, copy the installer files to a USB drive or a non-system partition so they are accessible immediately after Windows reinstall, before connecting to the internet.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Copy the downloaded driver installer (.exe) files to a USB flash drive or a secondary drive/partition (e.g. D:). This ensures you can install drivers before going online on the fresh Windows installation.

#### 0.2 Chipset & Network Drivers

A subsection of Max.mov's guide.

- **Intel VMD / RST note: disks may not appear during install** (`intel-vmd-rst-note` · manual step · from Max.mov's guide)
  - Description: If your SSD does not appear in the Windows installer disk selection screen, disable the Intel VMD controller in BIOS before installing. Alternatively, load the Intel RST driver during setup. See instruction for details.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: If disks are missing during Windows setup: Option A — Disable Intel VMD in BIOS (NVMe RAID / Intel RST setting) before installing. Option B — Extract RST driver from SetupRST.exe using: ./SetupRST.exe -extractdrivers SetupRST_extracted, then copy the extracted folder to a USB drive and load it via "Load Driver" during setup. Intel chipset drivers for modern Intel platforms are delivered via Windows Update automatically after first internet connection.
- **Download AMD chipset driver** (`download-amd-chipset` · link)
  - Description: Official AMD driver page. Chipset drivers for AMD platforms are available here.
  - What it does: Opens the site in your browser: `https://www.amd.com/en/support/download/drivers.html`
  - Buttons: VERSIONS, SITE
- **Download Intel chipset INF driver (older chipsets only, site availability varies by region)** (`download-intel-chipset-old` · link)
  - Description: Intel chipset INF utility for older Intel platforms. Modern Intel chipsets receive updates exclusively via Windows Update.
  - What it does: Opens the site in your browser: `https://www.intel.com/content/www/us/en/download/19347/chipset-inf-utility.html`
  - Buttons: VERSIONS, SITE
- **Download Intel RST driver** (`download-intel-rst` · link)
  - Description: Intel Rapid Storage Technology driver. Required only if your SSD is not detected during Windows setup and you need VMD/RST support.
  - What it does: Opens the site in your browser: `https://www.intel.com/content/www/us/en/search.html?ws=text#sort=relevancy&layout=table&f:downloadtype=[Drivers]&f:@operatingsystem_en=[Windows%2011%20Family*]&f:@tabfilter=[Downloads]&f:@stm_10385_en=[Memory%20and%20Storage]`
  - Buttons: VERSIONS, SITE
- **Download drivers from motherboard manufacturer website** (`motherboard-support-page` · manual step)
  - Description: Visit your motherboard manufacturer support page (ASUS, MSI, Gigabyte, ASRock) to download the latest chipset and LAN drivers for your specific board model.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Search for your motherboard model on the manufacturer website (e.g. asus.com/support, msi.com/support). Download: (1) chipset driver, (2) LAN/Wi-Fi driver. Copy them to your USB drive along with GPU drivers.
- **Broadcom / Realtek / Intel Killer network drivers (other manufacturers)** (`network-other-manufacturers` · manual step)
  - Description: If your network adapter is from Broadcom, Realtek, or Intel Killer and is not recognized after install, search for the driver on the manufacturer website or use the motherboard support page.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: If Windows Update does not install your LAN/Wi-Fi driver automatically, download it from: Realtek — realtek.com, Broadcom — broadcom.com, Intel Killer — killer.intel.com. Or search for it by adapter model in Device Manager.

#### 0.3 Create Installation Media

- **Download Windows 11 Installation Media (Media Creation Tool, availability varies by region)** (`download-win11-mct` · link · from Max.mov's guide)
  - Description: Official Microsoft page to download the Windows 11 Media Creation Tool, which creates a bootable USB installation drive automatically.
  - What it does: Opens the site in your browser: `https://www.microsoft.com/en-us/software-download/windows11`
  - Buttons: INSTALLER, SITE
- **Download official Windows 11 ISO image (availability varies by region)** (`download-win11-iso-official` · link · from Max.mov's guide)
  - Description: Direct ISO download from Microsoft for use with Rufus or manual installation without a USB drive. Microsoft does not offer it in every region.
  - What it does: Opens the site in your browser: `https://www.microsoft.com/en-us/software-download/windows11`
  - Buttons: OPEN
- **Download Windows 11 images via UUP Dump (available where Microsoft's download is not)** (`download-win11-uup-dump` · link · side road)
  - Description: Alternative source for Windows 11 ISO images including those built via UUP Dump. Works in regions where Microsoft does not offer the download.
  - What it does: Opens the site in your browser: `https://www.comss.ru/list.php?c=windows10_update`
  - Buttons: OPEN
- **Download Rufus (bootable USB creator)** (`download-rufus` · link · from Max.mov's guide · side road)
  - Description: Rufus creates bootable USB installation drives from ISO images. Use it to write the Windows 11 ISO onto a USB flash drive (8GB+ recommended).
  - What it does: Opens the site in your browser: `https://github.com/pbatard/rufus/releases`
  - Buttons: INSTALLER, PORTABLE, SITE
- **Create bootable USB installation drive** (`create-usb-drive` · manual step · **ESSENTIAL** · from Max.mov's guide)
  - Description: Use Rufus to write the Windows 11 ISO to a USB flash drive. In Rufus, select the ISO file, choose the target USB drive, and click Start. Use GPT partition scheme for UEFI systems.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: 1. Open Rufus. 2. Select your USB drive (8GB+). 3. Click "SELECT" and choose the Windows 11 ISO. 4. Partition scheme: GPT. Target system: UEFI (non-CSM). 5. File system: NTFS. 6. Click START. Wait for completion. The USB drive is now bootable.

#### 0.4 Install Without USB

Optional. A subsection of Max.mov's guide.

- **Note: not recommended if switching from Legacy to UEFI** (`no-usb-note` · manual step · from Max.mov's guide)
  - Description: Installing without a USB drive by creating a temporary 12GB partition works on most systems, but may not be suitable if you are also switching from Legacy BIOS to UEFI boot mode.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: If you are switching from Legacy/MBR to UEFI/GPT, use a USB drive instead. This method is best for a clean reinstall on an already-UEFI system.
- **Open Disk Management to create the install partition** (`open-disk-management-partition` · Windows page · administrator · from Max.mov's guide)
  - Description: Open Disk Management to shrink the system drive by 12288 MB and create a new simple volume for the Windows installation files.
  - What it does: Opens in Windows: `diskmgmt.msc`
  - Buttons: OPEN IN WINDOWS
  - Instruction: 1. Right-click the system drive (C:) → Shrink Volume. 2. Enter 12288 MB to shrink. 3. After shrink, right-click the new unallocated space → New Simple Volume. 4. Format as NTFS, label it "Win11". 5. If the shrink fails, see the troubleshooting note below.
- **Troubleshoot: cannot shrink volume (USN journal / pagefile)** (`no-usb-shrink-errors` · manual step · administrator · from Max.mov's guide)
  - Description: If Disk Management cannot shrink the volume, the USN journal or pagefile may be blocking it. Common errors: $Extend\$UsnJrnl (delete journal), pagefile.sys (disable pagefile), $Mft::$BITMAP (use a different drive or method).
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: To delete USN journal, open an elevated CMD and run: fsutil usn deletejournal /D C: — then recreate with: fsutil usn createjournal m=0 a=0 C:. To disable pagefile: System Properties → Advanced → Performance Settings → Advanced → Virtual Memory → No paging file. To disable System Protection: System Properties → System Protection → select C: → Configure → Disable. If $Mft::$BITMAP appears, use a different disk or installation method.
- **Check Event Viewer if shrink fails** (`open-event-viewer-shrink` · Windows page · from Max.mov's guide)
  - Description: If Disk Management refuses to shrink the volume and gives no clear reason, Event Viewer may show which file is blocking the operation.
  - What it does: Opens in Windows: `eventvwr.msc`
  - Buttons: OPEN IN WINDOWS
  - Instruction: In Event Viewer → Windows Logs → Application, look for events from source "defrag" around the time of the failed shrink attempt. The event text will show the file that is preventing the shrink.
- **No-USB install: CMD method (copy ISO files to Win11 partition)** (`no-usb-cmd-method` · manual step · restart · **CAREFUL** · from Max.mov's guide)
  - Description: Download the Windows 11 ISO, mount it in File Explorer (double-click), and copy all files to the 12GB Win11 partition. Then boot into it via Restart / Advanced Startup.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: 1. Download the Windows 11 ISO. 2. Double-click the ISO to mount it as a drive letter. 3. Copy ALL files from the mounted ISO to the Win11 partition (12GB). 4. To boot into it: open Restart advanced startup options. Alternatively reboot — most motherboards will detect and boot the Win11 partition automatically.
- **Download EasyBCD (no-USB method 2)** (`download-easybcd` · link · from Max.mov's guide · side road)
  - Description: EasyBCD adds a boot entry pointing to the Windows 11 installer WIM file on the Win11 partition, so the PC boots into the installer at next restart without a USB drive.
  - What it does: Opens the site in your browser: `https://neosmart.net/EasyBCD/`
  - Buttons: OPEN
- **No-USB install: EasyBCD method** (`no-usb-easybcd-method` · manual step · administrator · restart · **CAREFUL** · from Max.mov's guide · side road)
  - Description: Use EasyBCD to add a WinPE boot entry pointing to boot.wim in the Win11 partition. On next reboot, select the NST entry to launch the Windows 11 installer.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: 1. Download the ISO, mount it, copy all files to the Win11 partition. 2. In the Win11 partition, navigate to sources\ and copy the path to boot.wim. 3. Open EasyBCD → Add New Entry → WinPE tab. 4. Paste the path to boot.wim in the Path field. Click the + button. 5. Save and reboot. Select the NST entry in the boot menu to start the installer.

#### 0.5 Move Max.mov Archive to USB / Other Drive

- **Move the Audion Setup Tools by Max.mov archive and drivers to a USB or separate drive** (`move-archive-reminder` · manual step · from Max.mov's guide)
  - Description: Before reinstalling Windows, ensure the Audion Setup Tools by Max.mov folder and all downloaded driver installers are saved on a USB drive or a non-system partition (e.g. D:). They will be wiped if left on C:.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Copy the entire "Audion Setup Tools by Max.mov" folder and all driver installers to a USB flash drive or a secondary disk partition (NOT C:). Confirm they are accessible before proceeding with the Windows installation.

#### 0.6 BIOS Settings

A subsection of Max.mov's guide.

- **AMD note: virtualization may be called SVM or AMD-V** (`bios-amd-note` · manual step)
  - Description: On AMD systems, the virtualization setting is typically labeled SVM, SVM Mode, or AMD-V instead of Intel VT-x. Disable it to disable VBS / Defender Core Isolation if desired.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In BIOS, search for: SVM Mode, SVM, or AMD-V. This is the AMD equivalent of Intel VT-x virtualization. Disable it only if you do not use virtual machines, to allow disabling Windows VBS security features.
- **Disable manufacturer preinstalled software (MSI Center, ASUS Armoury Crate, etc.)** (`bios-disable-preinstalled-software` · manual step)
  - Description: Some motherboard manufacturers pre-install bloatware via BIOS (MSI Center, ASUS Armoury Crate). Disable this in BIOS before installing Windows to avoid unwanted software being installed automatically.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In BIOS, look for settings related to "APP Center Download & Install", "Armoury Crate", "MSI Center", or similar. Disable them to prevent automatic installation of manufacturer software on the fresh Windows install.
- **Disable CSM Support and enable Secure Boot** (`bios-csm-secureboot` · manual step · **CAREFUL**)
  - Description: Windows 11 requires UEFI with Secure Boot enabled. Disable CSM (Compatibility Support Module) fully and set Secure Boot to Enabled. Required for proper UEFI installation.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In BIOS: (1) Find CSM (Compatibility Support Module) and set it to Disabled. (2) Find Secure Boot and set it to Enabled. If Secure Boot cannot be enabled while CSM is on, disable CSM first, then enable Secure Boot. Save and reboot to confirm UEFI mode is active.
- **Enable TPM 2.0 (Intel PTT, AMD fTPM, or physical TPM module)** (`bios-tpm` · manual step)
  - Description: Windows 11 requires TPM 2.0. Enable it in BIOS — Intel calls it "Intel PTT", AMD calls it "AMD fTPM". Physical TPM modules also work.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In BIOS, find: Intel Platform Trust Technology (PTT) — set to Enabled, or AMD fTPM — set to Enabled. If you have a physical TPM module installed, enable "Discrete TPM" instead. Confirm TPM 2.0 is active in Windows via Win+R → tpm.msc after install.
- **Disable unused devices (audio, iGPU if not needed) — desktop only** (`bios-disable-unused-devices` · manual step)
  - Description: On desktop PCs, you can disable unused integrated devices (onboard audio, iGPU) in BIOS to reduce system load and potential driver conflicts. Not recommended for laptops.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In BIOS (desktop only): consider disabling Onboard Audio if you use a dedicated sound card, and disabling iGPU if you exclusively use a dedicated GPU and the display is not connected to integrated outputs. Do NOT disable iGPU on a laptop — the display depends on it.
- **Disable virtualization (if not needed) — disables VBS** (`bios-disable-virtualization` · manual step · side road)
  - Description: Disabling CPU virtualization (Intel VT-x / AMD SVM) in BIOS prevents Windows from enabling Virtualization Based Security (VBS), which has a minor performance impact on games. Only disable if you do not use VMs or WSL2.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In BIOS, find: Intel Virtualization Technology (VT-x) or AMD SVM Mode. Set to Disabled. This prevents Windows Defender Credential Guard and Core Isolation from using VBS, which can slightly improve game performance. Note: disabling this makes WSL2 and Hyper-V unavailable.
- **Disable unused drives in BIOS (if supported)** (`bios-disable-extra-drives` · manual step)
  - Description: If your BIOS supports disabling individual storage devices programmatically, disable drives you will not use during installation to simplify the disk selection screen.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In BIOS → Storage configuration, disable any drives that are not involved in the Windows installation (e.g. secondary data drives). This prevents accidentally selecting the wrong disk during setup. Re-enable them after installation is complete.
- **Enable PWM mode for 4-pin fans** (`bios-pwm-fans` · manual step)
  - Description: Set 4-pin fan headers to PWM mode (not DC/Voltage) in BIOS to allow proper RPM control by the cooling system. Required for accurate fan curve control.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In BIOS → Hardware Monitor / Fan Control, set each 4-pin fan connector to PWM mode. In DC mode, fan speed is controlled by voltage and is less precise. PWM mode allows exact RPM control via duty cycle.

#### 0.7 Important Notes Before Installation

A subsection of Max.mov's guide.

- **If PC reboots back to setup instead of OOBE — read this** (`oobe-loop-note` · manual step · from Max.mov's guide)
  - Description: After installation completes and reboots, some motherboards (especially MSI) fail to switch boot priority to the new Windows partition and loop back to the installer. Windows IS already installed — follow the instruction to resolve this without reinstalling.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: INSTALL FROM USB: Simply unplug the USB drive and restart. The PC will boot into the newly installed Windows and continue to OOBE. Reconnect the USB after reaching the desktop.

INSTALL WITHOUT USB (no-USB method): If you return to the install screen, try entering BIOS and changing the UEFI Hard Drive BBS Priorities to put the correct Windows partition first (e.g. MSI BIOS → Settings → Boot → UEFI Hard Drive BBS Priorities).

If BIOS does not allow changing partition priority (common on MSI laptops): press Shift+F10 at the install screen to open CMD. Run: diskpart → list vol → select vol N (where N is the ~12GB Win11 partition) → delete vol. Reboot. Windows will then boot from the installed partition. Note: this deletes the install partition — download any needed files from another PC or phone before reconnecting to the internet.

#### 0.8 New Driver Install Method & MS Account Bypass

A subsection of Max.mov's guide.

- **Watch video guide: new driver installation method during OOBE** (`new-method-video` · link · from Max.mov's guide)
  - Description: Video fragment demonstrating the new method of installing drivers during OOBE (out-of-box experience) before connecting to the internet, then bypassing the Microsoft account requirement.
  - What it does: Opens the site in your browser: `https://youtu.be/Itk_7yTI4PY?t=191`
  - Buttons: OPEN
- **Install drivers during OOBE (before internet, without MS account)** (`new-driver-method-steps` · manual step · from Max.mov's guide)
  - Description: During the Windows 11 initial setup screen (OOBE), press Shift+F10, type explorer.exe to open File Explorer, install chipset and GPU drivers from USB, then reboot back into OOBE. Connect to the internet when asked, but skip the Microsoft account using one of the methods below.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: 0. Stay offline until the drivers are in. 1. At the OOBE screen, press Shift+F10. 2. Type: explorer.exe and press Enter. 3. File Explorer opens — install the chipset driver and the GPU driver from your USB drive (the GPU driver as the guide does it: see the NVIDIA driver card); drives switched off in BIOS may need turning back on. 4. In CMD, type: shutdown.exe /r /t 00 to reboot back into OOBE. 5. Continue setup and connect to the internet when prompted. 6. To skip Microsoft account:
   — Method IV (Pro only): choose "work/school" → "Join domain" instead.
   — Method V+: enter aaa@gmail.com with a wrong password — after repeated failures, Windows offers a local account.
   — Method V: log into MS account, then go to Settings → Accounts → Your info → "Sign in with a local account instead".

#### 0.9 Start Installation

A subsection of Max.mov's guide.

- **Reboot and boot from USB installation drive** (`boot-from-usb` · Windows page · restart · from Max.mov's guide)
  - Description: Restart the PC and enter the boot menu (usually F8, F11, F12, or Del depending on motherboard) to select the USB drive as the boot device. The Windows 11 installer will start.
  - What it does: Opens in Windows: `ms-settings:recovery`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Option A: Hold Shift and click Restart → Troubleshoot → Advanced options → UEFI Firmware Settings to enter BIOS, then change boot order to USB. Option B: Restart normally and press F11/F12 (varies by board) at the manufacturer splash screen to open the one-time boot menu.
- **Boot into installation: no-USB CMD method (reboot into recovery mode)** (`boot-no-usb-cmd` · Windows page · restart · from Max.mov's guide)
  - Description: If using the no-USB CMD method: restart into Windows Recovery mode to access the Win11 install partition. See instruction.
  - What it does: Opens in Windows: `ms-settings:recovery`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Go to Settings → System → Recovery → Advanced startup → Restart now. From the recovery menu, choose "Use a device" and select the Win11 partition. Alternatively, hold Shift while clicking Restart.
- **Boot into installation: no-USB EasyBCD method (select NST entry on reboot)** (`boot-no-usb-easybcd` · Windows page · restart · from Max.mov's guide)
  - Description: If you used EasyBCD to add a WinPE boot entry, simply restart the PC and select the "NST" entry in the Windows Boot Manager to launch the installer.
  - What it does: Opens in Windows: `ms-settings:recovery`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Restart the PC. At the Windows Boot Manager screen, select the NST (NeoSmart Technologies) entry. This boots into the WinPE installer you configured with EasyBCD. Proceed with the normal Windows 11 installation from there.

### 1. Driver Installation

#### 1.0 Setup Notes

A subsection of Max.mov's guide.

- **v0.4 setup note — skip this section if drivers were installed during OOBE** (`setup-notes-v04` · manual step)
  - Description: If you used the new driver installation method via Explorer during OOBE (Section 0), skip this section — drivers and internet are already set up.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: If you installed drivers via the Explorer trick during Windows setup (Section 0 → Step 8), you may skip this section.
- **Open Device Manager** (`open-device-manager-drivers` · Windows page · from Max.mov's guide)
  - Description: Open Device Manager to verify GPU, chipset, storage, Wi-Fi, Bluetooth, and unknown devices after installing Windows.
  - What it does: Opens in Windows: `devmgmt.msc`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Check Display adapters, Network adapters, Bluetooth, Storage controllers, and Other devices. Unknown devices usually mean a missing chipset, Wi-Fi, Bluetooth, or storage driver.

#### 1.2 NVIDIA Drivers

A subsection of Max.mov's guide.

- **Download NVIDIA Game Ready driver** (`download-nvidia-driver` · link)
  - Description: The Game Ready branch for games: the latest one for this machine's card - desktop or notebook - from NVIDIA's driver service. The Studio driver is the next card; the site has every other driver.
  - What it does: Opens the site in your browser: `https://www.nvidia.com/en-us/drivers/`
  - Buttons: VERSIONS, SITE
- **Download NVIDIA Studio driver** (`download-nvidia-studio-driver` · link)
  - Description: The Studio branch: the same GeForce cards, releases tested longer with creative apps (DaVinci Resolve, Adobe, Blender). The latest one for this machine's card - desktop or notebook - from NVIDIA's driver service.
  - What it does: Opens the site in your browser: `https://www.nvidia.com/en-us/drivers/`
  - Buttons: VERSIONS, SITE
- **NVIDIA App — driver updates and game optimization** (`download-nvidia-app-drivers` · link)
  - Description: Official NVIDIA App page. Use it for driver updates, game optimization, overlays, DLSS Overrides, and GPU tuning.
  - What it does: Opens the site in your browser: `https://www.nvidia.com/en-us/software/nvidia-app/`
  - Buttons: INSTALLER, SITE
- **NVCleanstall — minimal NVIDIA driver installer** (`download-nvcleanstall` · link · side road)
  - Description: GUI wrapper that lets you install only the Display Driver component and optional PhysX/Audio. Cleaner than the official custom install. NVIDIA only.
  - What it does: Opens the site in your browser: `https://www.techpowerup.com/download/techpowerup-nvcleanstall/`
  - Buttons: PORTABLE, SITE
- **Install NVIDIA GPU driver (clean, no bloatware)** (`install-nvidia-driver` · manual step · administrator · restart · **ESSENTIAL** · from Max.mov's guide)
  - Description: Only the display driver, the way the guide does it: a shortcut to the NVIDIA installer with -passive Display.Driver - no NVIDIA App, no telemetry services.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: 1. Download the driver from nvidia.com/drivers, do not run it. 2. Make a shortcut to the downloaded .exe and add to its Target: -passive Display.Driver (first install and updates) or -passive -clean Display.Driver (also resets the driver settings). 3. Run the shortcut as administrator - the installer shows only a progress bar. 4. Restart. Then HDCP and Ansel off (GPU section). Alternative: NVCleanstall with only "Display Driver" ticked.
- **DDU — Display Driver Uninstaller** (`download-ddu` · link · side road)
  - Description: Completely removes NVIDIA/AMD/Intel GPU drivers and leftover registry entries. Use before switching GPU vendors or when a driver is corrupted. Run in Safe Mode.
  - What it does: Opens the site in your browser: `https://www.wagnardsoft.com/display-driver-uninstaller-DDU-`
  - Buttons: VERSIONS, SITE
- **Remove old GPU driver with DDU** (`remove-old-gpu-driver` · manual step · administrator · restart · **CAREFUL** · side road)
  - Description: Only needed when switching GPU vendor, fixing a corrupted driver, or cleaning persistent issues after an update. Not required on a clean Windows install.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: 1. Download DDU. 2. Boot into Safe Mode (hold Shift → Restart → Troubleshoot → Advanced → Startup Settings → Safe Mode with Networking). 3. Run DDU, select GPU type (NVIDIA/AMD/Intel), click "Clean and restart". 4. After reboot install the new driver from the vendor section.

#### 1.3 AMD Drivers

A subsection of Max.mov's guide.

- **Download official AMD graphics driver** (`download-amd-driver` · link)
  - Description: Official AMD Drivers and Support page. Use it for Radeon graphics, Ryzen processors with graphics, and the AMD auto-detect tool.
  - What it does: Opens the site in your browser: `https://www.amd.com/en/support/download/drivers.html`
  - Buttons: VERSIONS, SITE
- **Download official AMD chipset driver** (`download-amd-chipset-driver` · link)
  - Description: Official AMD Drivers and Support page. Chipset drivers for AMD desktop and laptop platforms are available here.
  - What it does: Opens the site in your browser: `https://www.amd.com/en/support/download/drivers.html`
  - Buttons: VERSIONS, SITE
- **Install AMD graphics/chipset drivers** (`install-amd-drivers` · manual step · administrator · restart · **ESSENTIAL**)
  - Description: Install AMD graphics and chipset packages from the official AMD page or from your motherboard/laptop support page.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: For Radeon graphics: use AMD Drivers and Support or AMD Auto-Detect. For AMD chipset: select Chipsets on the AMD page or use the motherboard/laptop support page. Reboot after installation.

#### 1.4 Intel Drivers

A subsection of Max.mov's guide.

- **Install Intel Driver & Support Assistant via winget** (`install-intel-dsa-winget` · script · administrator)
  - Description: Installs Intel Driver & Support Assistant through winget to detect Intel graphics, Wi-Fi, Bluetooth, chipset, and storage updates.
  - What it does: Installs or updates via winget: `Intel.IntelDriverAndSupportAssistant`.
  - Buttons: INSTALL / UPDATE
  - Note: Installs or updates Intel DSA via winget.
- **Intel Driver & Support Assistant — official page** (`download-intel-dsa` · link)
  - Description: Official Intel auto-detect utility. It provides a curated list of available updates for identified Intel products.
  - What it does: Opens the site in your browser: `https://www.intel.com/content/www/us/en/support/detect.html`
  - Buttons: INSTALLER, SITE
- **Download Intel Arc / integrated graphics driver** (`download-intel-graphics-driver` · link)
  - Description: Official Intel Arc and Intel integrated graphics driver page for Windows.
  - What it does: Opens the site in your browser: `https://www.intel.com/content/www/us/en/download/785597/intel-arc-graphics-windows.html`
  - Buttons: VERSIONS, SITE
- **Download Intel Chipset INF (Chipset Device Software)** (`download-intel-chipset-inf-driver` · link)
  - Description: Every Intel chipset package from 10.0.13 on. The Intel devices of this PC pick the one that carries their INF - old platforms (Sandy Bridge to Broadwell, X79/X99) are only in 10.1.18981.6008, older Xeon Scalable in the Server package. The INF mostly names the devices in Device Manager; modern chipsets also get it from Windows Update.
  - What it does: Opens the site in your browser: `https://www.intel.com/content/www/us/en/download/19347/chipset-inf-utility.html`
  - Buttons: VERSIONS, SITE
- **Download Intel Management Engine driver** (`download-intel-me-driver` · link)
  - Description: The driver of the Intel ME interface (HECI) as Windows Update gives it: from the Microsoft Update Catalog by this PC's device id, WHQL, without Intel's programs. The installed version is marked; usually Windows Update has put it already.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=Intel%20Management%20Engine%20Interface`
  - Buttons: VERSIONS, SITE
- **Download Intel RST driver** (`download-intel-rst-driver` · link)
  - Description: Intel Rapid Storage Technology driver. Required only if your SSD is not detected during Windows setup or your system uses VMD/RST.
  - What it does: Opens the site in your browser: `https://www.intel.com/content/www/us/en/download/15667/intel-rapid-storage-technology-intel-rst-driver-installation-software-with-intel-optane-memory.html`
  - Buttons: VERSIONS, SITE

#### 1.5 Qualcomm Snapdragon Drivers

- **Download Qualcomm Snapdragon X graphics driver** (`download-snapdragon-graphics-driver` · link)
  - Description: The Adreno graphics driver of Snapdragon X laptops, every released version from TechPowerUp; ticked only on a Snapdragon PC.
  - What it does: Opens the site in your browser: `https://www.techpowerup.com/download/qualcomm-snapdragon-x-graphics-drivers/`
  - Buttons: VERSIONS, SITE

#### 1.6 Wi-Fi & Network Drivers

A subsection of Max.mov's guide.

- **Open motherboard / laptop support page** (`motherboard-laptop-support-page` · manual step)
  - Description: Download chipset, LAN, Wi-Fi, Bluetooth, and storage drivers from the exact motherboard or laptop manufacturer support page.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Search your exact motherboard or laptop model on ASUS/MSI/Gigabyte/ASRock/Lenovo/HP/Dell support. Download chipset, LAN/Wi-Fi, Bluetooth, audio, and storage drivers matching your Windows version.
- **Install chipset and network drivers** (`install-chipset-network-driver` · manual step · administrator · restart · **ESSENTIAL**)
  - Description: Download and install the motherboard chipset driver and any remaining network/LAN drivers from the motherboard manufacturer website.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Visit your motherboard manufacturer website (e.g. ASUS, MSI, Gigabyte, ASRock), locate your board model, download the chipset and network drivers, install them in order. Vendor links below are additional shortcuts for common Wi-Fi adapter makers.
- **Realtek Wi-Fi adapter drivers** (`download-realtek-wifi-drivers` · link)
  - Description: Official Realtek Wireless LAN IC downloads page. Use it when the adapter model is Realtek and Windows Update/OEM support page did not provide a newer driver.
  - What it does: Opens the site in your browser: `https://www.realtek.com/Download/Index?cate_id=203&menu_id=297`
  - Buttons: VERSIONS, SITE
- **Realtek LAN drivers (PCIe GbE / 2.5GbE)** (`download-realtek-lan-drivers` · link)
  - Description: The driver of a Realtek network card. From the Microsoft Update Catalog by this PC's device id: WHQL, without the vendor's programs; the installed version is marked.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=Realtek%20PCIe%20GbE%20Family%20Controller`
  - Buttons: VERSIONS, SITE
- **Realtek Bluetooth drivers** (`download-realtek-bluetooth-drivers` · link)
  - Description: The driver of a Realtek Bluetooth adapter. From the Microsoft Update Catalog by this PC's device id: WHQL, without the vendor's programs; the installed version is marked.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=Realtek%20Bluetooth`
  - Buttons: VERSIONS, SITE
- **Intel Wireless Wi-Fi drivers** (`download-intel-wifi-drivers` · link)
  - Description: Official Intel Wi-Fi driver package for Windows 10 and Windows 11 wireless adapters.
  - What it does: Opens the site in your browser: `https://www.intel.com/content/www/us/en/download/19351/intel-wireless-wi-fi-drivers-for-windows-10-and-windows-11.html`
  - Buttons: VERSIONS, SITE
- **Intel Wireless Bluetooth drivers** (`download-intel-bluetooth-drivers` · link)
  - Description: Intel Bluetooth driver package for Windows 10 and Windows 11 - the companion of the Intel Wi-Fi driver on the same combo card.
  - What it does: Opens the site in your browser: `https://www.intel.com/content/www/us/en/download/18649/intel-wireless-bluetooth-drivers-for-windows-10-and-windows-11.html`
  - Buttons: VERSIONS, SITE
- **Intel Ethernet network drivers** (`download-intel-ethernet-drivers` · link)
  - Description: Intel wired network drivers (I219, I225, I226 and others) for Windows 10 and Windows 11. The package comes zipped and is unpacked beside it.
  - What it does: Opens the site in your browser: `https://www.intel.com/content/www/us/en/download/18293/intel-network-adapter-driver-for-windows-10.html`
  - Buttons: VERSIONS, SITE
- **MediaTek / MTK Wi-Fi driver guidance** (`download-mediatek-wifi-info` · link)
  - Description: Official MediaTek networking page. For most modern laptop Wi-Fi adapters, MediaTek directs end users to the device manufacturer support page or Windows Update.
  - What it does: Opens the site in your browser: `https://www.mediatek.com/products/networking-and-connectivity`
  - Buttons: OPEN
- **MediaTek Wi-Fi drivers — Microsoft Update Catalog** (`download-mediatek-wifi-catalog` · link)
  - Description: Microsoft Update Catalog search for MediaTek MT7921/MT7922 Wi-Fi drivers. Useful when the OEM page is outdated or unavailable.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=MediaTek%20Wi-Fi%206%20MT7921%20Wireless%20LAN%20Card`
  - Buttons: VERSIONS, SITE
- **MediaTek Bluetooth drivers** (`download-mediatek-bluetooth-drivers` · link)
  - Description: The driver of a MediaTek Bluetooth adapter (the pair of MT7921/MT7922 Wi-Fi). From the Microsoft Update Catalog by this PC's device id: WHQL, without the vendor's programs; the installed version is marked.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=MediaTek%20Bluetooth`
  - Buttons: VERSIONS, SITE
- **Qualcomm Wi-Fi drivers (FastConnect, Atheros, Killer)** (`download-qualcomm-wifi-drivers` · link)
  - Description: The driver of a Qualcomm Wi-Fi adapter. From the Microsoft Update Catalog by this PC's device id: WHQL, without the vendor's programs; the installed version is marked.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=Qualcomm%20Wi-Fi`
  - Buttons: VERSIONS, SITE
- **Qualcomm Bluetooth drivers** (`download-qualcomm-bluetooth-drivers` · link)
  - Description: The driver of a Qualcomm Bluetooth adapter. From the Microsoft Update Catalog by this PC's device id: WHQL, without the vendor's programs; the installed version is marked.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=Qualcomm%20Bluetooth`
  - Buttons: VERSIONS, SITE
- **Broadcom Wi-Fi drivers** (`download-broadcom-wifi-drivers` · link)
  - Description: The driver of a Broadcom Wi-Fi adapter (BCM43xx, in older laptops and Macs). From the Microsoft Update Catalog by this PC's device id: WHQL, without the vendor's programs; the installed version is marked.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=Broadcom%20Wireless`
  - Buttons: VERSIONS, SITE
- **Broadcom Bluetooth drivers** (`download-broadcom-bluetooth-drivers` · link)
  - Description: The driver of a Broadcom Bluetooth adapter; the catalog has only an old one, Windows's own driver usually does. From the Microsoft Update Catalog by this PC's device id: WHQL, without the vendor's programs; the installed version is marked.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=Broadcom%20Bluetooth`
  - Buttons: VERSIONS, SITE
- **Broadcom LAN drivers (NetXtreme)** (`download-broadcom-lan-drivers` · link)
  - Description: The driver of a Broadcom NetXtreme network card (desktops, workstations, servers). From the Microsoft Update Catalog by this PC's device id: WHQL, without the vendor's programs; the installed version is marked.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=Broadcom%20NetXtreme`
  - Buttons: VERSIONS, SITE
- **Marvell AQtion LAN drivers (Aquantia 5G / 10G)** (`download-aqtion-lan-drivers` · link)
  - Description: The driver of a Marvell AQtion network card - the Aquantia AQC107/AQC113 5 and 10 Gbit chips on ASUS, Gigabyte and MSI boards and 10G cards. From the Microsoft Update Catalog by this PC's device id: WHQL, without the vendor's programs; the installed version is marked.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=Marvell%20AQtion`
  - Buttons: VERSIONS, SITE
- **Killer Ethernet drivers (E2xxx, E3xxx)** (`download-killer-ethernet-drivers` · link)
  - Description: The driver of a Killer network card: E2xxx are Qualcomm Atheros chips, E3000/E3100 are Realtek 2.5G. Bare driver, without Killer Control Center. From the Microsoft Update Catalog by this PC's device id: WHQL, without the vendor's programs; the installed version is marked.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=Killer%20Ethernet`
  - Buttons: VERSIONS, SITE
- **Killer Wi-Fi drivers** (`download-killer-wifi-drivers` · link)
  - Description: The driver of a Killer Wi-Fi adapter: AX1650/1675/1690 and newer are Intel chips (the Intel Wi-Fi card fits them too), 1435/1535 are Qualcomm Atheros. Killer Bluetooth is Intel Bluetooth. From the Microsoft Update Catalog by this PC's device id: WHQL, without the vendor's programs; the installed version is marked.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=Killer%20Wi-Fi`
  - Buttons: VERSIONS, SITE
- **Enable Wi-Fi or connect Ethernet cable** (`enable-network` · Windows page · from Max.mov's guide)
  - Description: Open advanced network settings to verify and configure your network adapter after driver installation.
  - What it does: Opens in Windows: `ms-settings:network-advancedsettings`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Ensure your network adapter is listed and enabled. Connect via Ethernet or toggle Wi-Fi on.

#### 1.7 Audio Drivers

- **Download Realtek audio driver** (`download-realtek-audio-driver` · link)
  - Description: The driver of the Realtek audio codec, as Windows Update gives it: the exact driver of this PC (its SUBSYS) first, then the codec's. From the Microsoft Update Catalog by this PC's device id: WHQL, without the vendor's programs; the installed version is marked. The Realtek Audio Console comes from the Microsoft Store.
  - What it does: Opens the site in your browser: `https://www.catalog.update.microsoft.com/Search.aspx?q=Realtek%20High%20Definition%20Audio`
  - Buttons: VERSIONS, SITE

### 2. System Update

#### 2.1 System, Runtime & Reboot

- **Set PC name** (`set-pc-name` · Windows page · administrator · restart · from Max.mov's guide)
  - Description: Open About settings to rename this PC. A restart is required for the name to take effect.
  - What it does: Opens in Windows: `ms-settings:about`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Click "Rename this PC", enter your preferred name, and restart when prompted.
- **Check for Windows Updates** (`windows-update` · Windows page · **ESSENTIAL** · from Max.mov's guide)
  - Description: Install all available Windows Updates before proceeding with further configuration.
  - What it does: Opens in Windows: `ms-settings:windowsupdate`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Click "Check for updates" and install everything available. Restart when prompted.
- **Check optional driver updates** (`windows-optional-driver-updates` · Windows page · from Max.mov's guide)
  - Description: Open Windows optional updates to review driver updates that are not delivered through the main Windows Update flow.
  - What it does: Opens in Windows: `ms-settings:windowsupdate-optionalupdates`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Open Driver updates, review every item, and install only drivers that match your current hardware.
- **Install Visual C++ Redistributables (official)** (`install-vcr-official` · link · from Max.mov's guide)
  - Description: Download the latest Visual C++ Redistributable packages from Microsoft. Required by many applications and games.
  - What it does: Opens the site in your browser: `https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist`
  - Buttons: INSTALL, INSTALLER, SITE
- **Install / Update Visual C++ 2015-2022 Redistributables via winget** (`install-vcr-winget` · script · administrator · **ESSENTIAL**)
  - Description: Installs or updates the official Microsoft Visual C++ 2015-2022 Redistributable packages (x64 + x86) sequentially through winget.
  - What it does: Installs or updates via winget: `Microsoft.VCRedist.2015+.x64`, `Microsoft.VCRedist.2015+.x86`.
  - Buttons: INSTALL / UPDATE
  - Note: Installs or updates both x64 and x86 packages sequentially via winget.
- **Install Visual C++ Redistributables 2005-2022 (all-in-one pack)** (`install-vcr-all-in-one` · link · from Max.mov's guide · side road)
  - Description: Alternative all-in-one pack covering VCR versions 2005 through 2022. Convenient single-installer option.
  - What it does: Opens the site in your browser: `https://www.techpowerup.com/download/visual-c-redistributable-runtime-package-all-in-one/`
  - Buttons: VERSIONS, SITE
- **Visual C++ Redistributable AIO — GitHub releases (abbodi1406)** (`install-vcr-aio-github` · link · side road)
  - Description: Another route to the same full package: the abbodi1406/vcredist releases page. Community-maintained, published openly on GitHub with checksums and a visible release history, so it is easy to verify what you are downloading. Use it when the TechPowerUp mirror is unavailable or you prefer the original source.
  - What it does: Opens the site in your browser: `https://github.com/abbodi1406/vcredist/releases`
  - Buttons: INSTALL, INSTALLER, SITE
- **⚠ Restart Windows now (60 second timer)** (`reboot-now-warning` · script · restart · **CAREFUL**)
  - Description: Schedules a Windows restart in 60 seconds after drivers and updates are installed. Use the cancel button if clicked by mistake.
  - What it does: A script from the manifest. Its state is checked.
  - Buttons: RESTART
  - Note: Alternative direct action: schedules an actual Windows restart, not just a checklist mark.
- **Cancel scheduled restart** (`cancel-scheduled-restart` · script)
  - Description: Cancels a pending shutdown.exe restart timer if one was scheduled from this tool or manually.
  - What it does: A script from the manifest. Its state is checked.
  - Buttons: CANCEL
  - Note: Alternative direct action: aborts a pending shutdown.exe restart timer.
- **Restart after all updates are installed** (`reboot-after-updates` · manual step · restart · from Max.mov's guide)
  - Description: Perform a clean restart after all drivers and Windows Updates have been applied.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Once all updates and drivers are installed, restart the PC: Start → Power → Restart. The sand restart button above is an optional direct shortcut.

### 3. Disk Preparation

#### 3.0 Partition Cleanup

A subsection of Max.mov's guide.

- **Delete Windows installation files partition (no-USB method)** (`delete-install-partition` · manual step · administrator · **CAREFUL**)
  - Description: After installing Windows without a USB drive, a temporary partition remains. Open Disk Management to delete it.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Open Disk Management (diskmgmt.msc), locate the small partition containing installation files, right-click it and select "Delete Volume". This only applies if you installed Windows using the no-USB method.
- **MiniTool Partition Wizard — advanced partition manager** (`minitool-partition-wizard` · link · side road)
  - Description: Free partition manager with a visual interface for more complex partition operations.
  - What it does: Opens the site in your browser: `https://www.partitionwizard.com/free-partition-manager.html`
  - Buttons: OPEN
- **Reconnect previously disconnected drives** (`reconnect-drives` · manual step)
  - Description: If you disconnected additional drives during Windows installation to prevent data loss, now reconnect them.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Power off the PC, reconnect any drives you disconnected before installation, then power on.

#### 3.1 Drive Letters & Explorer

A subsection of Max.mov's guide.

- **Verify all drives are visible in Explorer** (`verify-drives-explorer` · manual step)
  - Description: Open File Explorer and confirm all connected drives appear as expected.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Open File Explorer (Win+E) and check that all installed drives are listed under "This PC".
- **Assign drive letters (if drives are missing or mixed up)** (`assign-drive-letters` · manual step · administrator)
  - Description: Open Disk Management to assign or change drive letters if drives are not visible or have incorrect letters.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Open Disk Management (diskmgmt.msc), right-click the volume, choose "Change Drive Letter and Paths", and assign the desired letter.

#### 3.2 User Folder Relocation

A subsection of Max.mov's guide.

- **Copy User folder to a second partition or drive** (`copy-user-folder` · manual step)
  - Description: Move personal files (Desktop, Documents, Downloads, Music, Pictures, Videos) to a secondary drive to keep the OS drive clean and protect data across reinstalls.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Copy the User template folder to your second drive (e.g. D:\User). Inside create subfolders: Desktop, Documents, Downloads, Music, Pictures, Videos.
- **Redirect user shell folders to the new location** (`redirect-user-folders` · manual step)
  - Description: Open the Users folder in Explorer, then right-click each shell folder (Desktop, Documents, etc.) → Properties → Location to redirect them to the new drive.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Navigate to C:\Users\YourName\. Right-click Desktop → Properties → Location tab → Move → select D:\User\Desktop. Repeat for Documents, Downloads, Music, Pictures, Videos.
- **Relocate user folders to C:\<username>\ (script)** (`relocate-user-folders-to-root` · script · **CAREFUL** · side road)
  - Description: Creates C:\<username>\Desktop|Documents|Music|Pictures|Videos and writes legacy User Shell Folders registry keys. On Windows 11 the KnownFolders API may override these; sign out and back in to let Explorer pick up the change. Marked unofficial — test before relying on it.
  - What it does: A script from the manifest. Its state is checked. It has a way back.
  - Buttons: APPLY, REVERT
  - Instruction: Click Apply to create C:\<username>\ subfolders and redirect Desktop, Documents, Music, Pictures, Videos. A sign-out or Explorer restart is needed for all apps to pick up the new paths. Click Revert to restore default %USERPROFILE%\ paths.

#### 3.3 Apps & Gaming Folders

A subsection of Max.mov's guide.

- **Set up Apps folder for portable applications** (`copy-apps-folder` · manual step)
  - Description: Create an Apps folder on a secondary drive for portable and manually installed applications, keeping them separate from the OS drive.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Create a folder such as D:\Apps on your secondary drive. Install portable applications there to keep them safe across OS reinstalls.
- **Set up Gaming folder for games and launchers (optional)** (`copy-gaming-folder` · manual step)
  - Description: Create a Gaming folder on a secondary drive and configure Steam and other launchers to install games there by default.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Create D:\Gaming on your secondary drive. In Steam: Settings → Storage → Add Drive → select D:\Gaming. Configure other launchers similarly.

### 4. Browser Setup

#### 4.0 Install Browser

- **Google Chrome** (`install-chrome` · script)
  - Description: The most widely used browser. Best compatibility, V8 engine, sync across devices. Installs via winget.
  - What it does: Installs or updates via winget: `Google.Chrome`. REMOVE uninstalls it.
  - Buttons: INSTALL, OPEN SITE, REMOVE
- **Brave Browser** (`install-brave` · script)
  - Description: Chromium-based, built-in ad/tracker blocking, no Google telemetry. Good for privacy. Installs via winget.
  - What it does: Installs or updates via winget: `Brave.Brave`. REMOVE uninstalls it.
  - Buttons: INSTALL, OPEN SITE, REMOVE
- **Mozilla Firefox** (`install-firefox` · script)
  - Description: Independent Gecko engine (not Chromium). Strong privacy defaults, excellent extension ecosystem. Installs via winget.
  - What it does: Installs or updates via winget: `Mozilla.Firefox`. REMOVE uninstalls it.
  - Buttons: INSTALL, OPEN SITE, REMOVE
- **Vivaldi** (`install-vivaldi` · script)
  - Description: Chromium-based, extreme UI customisation, built-in tab groups, notes, mail. Power users. Installs via winget.
  - What it does: Installs or updates via winget: `Vivaldi.Vivaldi`. REMOVE uninstalls it.
  - Buttons: INSTALL, OPEN SITE, REMOVE
- **Opera** (`install-opera` · script · side road)
  - Description: Chromium-based with built-in VPN, ad blocker, and sidebar workspace tools. Installs via winget.
  - What it does: Installs or updates via winget: `Opera.Opera`. REMOVE uninstalls it.
  - Buttons: INSTALL, OPEN SITE, REMOVE
- **WebView2 Runtime (standalone — required if removing Edge)** (`webview2-standalone` · script · **ESSENTIAL** · from Max.mov's guide)
  - Description: Microsoft WebView2 Runtime powers PWAs, Teams, new Outlook, and some Store apps. Install before removing Edge to avoid breaking them. Windows 11 usually ships it already — the card then reports it as present and installs nothing.
  - What it does: Installs or updates via winget: `Microsoft.EdgeWebView2Runtime`. REMOVE uninstalls it.
  - Buttons: INSTALL, INSTALLER, SITE, REMOVE
- **Set default browser** (`set-default-browser` · Windows page · from Max.mov's guide)
  - Description: Open Default Apps settings to choose which browser handles http:// and https:// links.
  - What it does: Opens in Windows: `ms-settings:defaultapps`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Scroll to the browser section or search for your browser. Click it and choose "Set as default". Make sure http and https both point to your chosen browser.

#### 4.1 Edge Configuration

A subsection of Max.mov's guide.

- **Configure Microsoft Edge settings** (`configure-edge` · Windows page)
  - Description: Open Edge settings page to disable telemetry, personalisation, shopping features, and configure startup behaviour.
  - What it does: Opens in Windows: `microsoft-edge:settings/privacy`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Disable all telemetry and personalisation toggles. Under Privacy, search, and services → disable Help improve Microsoft products. Under New tab page → turn off news feed.
- **Configure sound devices & default playback** (`configure-sound` · manual step)
  - Description: Open Windows Sound control panel to set default playback and recording devices.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Open Sound control panel (mmsys.cpl), set your default playback device, configure levels. Disable unused playback/recording devices.
- **Browser setup guide (YouTube)** (`browser-setup-guide` · link · side road)
  - Description: Video guide covering browser configuration, privacy settings, and useful extensions.
  - What it does: Opens the site in your browser: `https://www.youtube.com/watch?v=ITdecD6R0Yw`
  - Buttons: OPEN

#### 4.2 Edge — Tame It

A subsection of Max.mov's guide. Switches: 7 - the subsection head has APPLY ALL and REVERT ALL.

- **Disable startup boost & background running** (`edge-disable-startup-boost` · registry switch · administrator)
  - Description: Prevents Edge from pre-launching at login and running in the background when all windows are closed.
  - What it does: Writes the registry: `HKLM\SOFTWARE\Policies\Microsoft\Edge` / `StartupBoostEnabled` = `0` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Disable background mode when Edge is closed** (`edge-disable-background` · registry switch · administrator)
  - Description: Stops Edge from staying active in background for notifications and extensions after all windows are closed.
  - What it does: Writes the registry: `HKLM\SOFTWARE\Policies\Microsoft\Edge` / `BackgroundModeEnabled` = `0` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Disable news feed on new tab page** (`edge-disable-newstab` · registry switch · administrator)
  - Description: Removes the Microsoft News / Bing content feed from Edge new tab page.
  - What it does: Writes the registry: `HKLM\SOFTWARE\Policies\Microsoft\Edge` / `NewTabPageContentEnabled` = `0` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Disable telemetry & diagnostic data collection** (`edge-disable-telemetry` · script · administrator)
  - Description: Sets three policy keys to stop Edge sending metrics, site info, and diagnostic data to Microsoft.
  - What it does: A script from the manifest. Its state is checked. It has a way back.
  - Buttons: APPLY, REVERT
- **Disable Shopping Assistant (price comparison popups)** (`edge-disable-shopping` · registry switch · administrator)
  - Description: Turns off the Edge Shopping Assistant that shows price comparisons and coupon suggestions on retail sites.
  - What it does: Writes the registry: `HKLM\SOFTWARE\Policies\Microsoft\Edge` / `EdgeShoppingAssistantEnabled` = `0` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Disable Microsoft Rewards in Edge** (`edge-disable-rewards` · registry switch · administrator)
  - Description: Removes Microsoft Rewards points integration and prompts from Edge UI.
  - What it does: Writes the registry: `HKLM\SOFTWARE\Policies\Microsoft\Edge` / `ShowMicrosoftRewards` = `0` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Disable first-run experience & import prompts** (`edge-disable-firstrun` · registry switch · administrator)
  - Description: Skips the Edge welcome/import wizard that appears on fresh installs or updates.
  - What it does: Writes the registry: `HKLM\SOFTWARE\Policies\Microsoft\Edge` / `HideFirstRunExperience` = `1` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)

#### 4.3 Remove Edge

Optional. A subsection of Max.mov's guide. Steps in order: do the cards top to bottom.

- **Read before proceeding — WebView2 dependency** (`remove-edge-webview2-warning` · manual step · **CAREFUL**)
  - Description: Removing Edge without a standalone WebView2 Runtime will break: Teams, new Outlook, PWA apps, Windows Widgets. Install it first via Install Browser → WebView2 Runtime.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Make sure "WebView2 Runtime (standalone)" is already installed (Install Browser subsection → Apply). Only then continue with the steps below.
- **Step 1 — Check if Uninstall is already available** (`remove-edge-step1-check` · Windows page)
  - Description: Open Apps & Features and look for Microsoft Edge. If the Uninstall button is active (not greyed out), skip to Step 6 and uninstall directly.
  - What it does: Opens in Windows: `ms-settings:appsfeatures`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Search "Microsoft Edge". If Uninstall is clickable — use it, done. If greyed out — follow Steps 2–6.
- **Step 2 — Grant write access to region policy file** (`remove-edge-step2-takeown` · script · administrator · **CAREFUL** · side road)
  - Description: Runs takeown and icacls on IntegratedServicesRegionPolicySet.json to allow editing. Required because the file is owned by TrustedInstaller.
  - What it does: A script from the manifest. Its state is checked.
  - Buttons: INSTALL
- **Step 3 — Open policy file in Notepad** (`remove-edge-step3-open-json` · script · side road)
  - Description: Opens IntegratedServicesRegionPolicySet.json in Notepad for manual editing. Run Step 2 first.
  - What it does: A script from the manifest. No state check: the button just runs it.
  - Buttons: RUN
- **Step 4 — Find the Edge entry and enable uninstall** (`remove-edge-step4-edit-json` · manual step · **CAREFUL** · side road)
  - Description: In the JSON file: find the "MicrosoftEdge" entry. In its "regions" array, add your 2-letter country code (e.g. "RU", "US", "DE"). Save the file with Ctrl+S.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In Notepad, press Ctrl+F and search for "MicrosoftEdge". Find the nearest "regions" array (usually looks like "regions":[""] or similar). Add your 2-letter country code inside the array, e.g.: "regions":["RU"]. Save the file with Ctrl+S, then close Notepad.
- **Step 5 — Click Repair on Edge (reloads policy)** (`remove-edge-step5-repair` · Windows page · administrator · side road)
  - Description: In Apps & Features, click the three-dot menu on Microsoft Edge → Modify. This forces Windows to reload the region policy and should unlock the Uninstall button.
  - What it does: Opens in Windows: `ms-settings:appsfeatures`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Find Microsoft Edge → click ⋮ → Modify. Wait for the repair to complete. Close this window completely (including any Edge processes).
- **Step 6 — Uninstall Edge** (`remove-edge-step6-uninstall` · Windows page · administrator · **CAREFUL** · side road)
  - Description: Reopen Apps & Features — the Uninstall button for Edge should now be active. Click it to remove Edge.
  - What it does: Opens in Windows: `ms-settings:appsfeatures`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Search for "Microsoft Edge". Click ⋮ → Uninstall. If still greyed out, close and reopen Settings, or restart the PC and try again.

### 5. Windows Settings

#### 5.0 Explorer Settings

A subsection of Max.mov's guide.

- **Remove item-selection checkboxes & clear history** (`explorer-remove-checkboxes` · Windows page)
  - Description: Uncheck three checkboxes in Folder Options and clear recent-files log.
  - What it does: Opens in Windows: `shell:::{ED7BA470-8E54-465E-825C-99712043E01C}`
  - Buttons: OPEN IN WINDOWS
  - Instruction: In Folder Options uncheck: Show recently used files, Show frequently used folders, Show files from Office.com. Then click Clear under Recent files.
- **Configure Explorer view options** (`explorer-configure` · Windows page)
  - Description: Open Explorer Options to set default folder, file-name extensions, hidden files, etc.
  - What it does: Opens in Windows: `shell:::{ED7BA470-8E54-465E-825C-99712043E01C}`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Set view preferences and show hidden/system files as desired.
- **PowerToys — keyboard shortcut remapping** (`explorer-powertoys-keybindings` · link)
  - Description: Download Microsoft PowerToys for advanced keyboard shortcuts and utilities.
  - What it does: Opens the site in your browser: `https://github.com/microsoft/PowerToys/releases`
  - Buttons: INSTALL, INSTALLER, SITE
- **Auto-size columns (CTRL + Numpad *)** (`explorer-autosize-columns` · manual step)
  - Description: Keyboard shortcut CTRL+(Numpad *) resizes all columns to fit content in Details view.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In any Explorer window in Details view, press CTRL + * (numpad asterisk) to auto-size all columns.

#### 5.1 System

A subsection of Max.mov's guide. Switches: 4 - the subsection head has APPLY ALL and REVERT ALL.

- **Display — resolution, scale, refresh rate** (`system-display` · Windows page)
  - Description: Set correct resolution and maximum monitor refresh rate.
  - What it does: Opens in Windows: `ms-settings:display`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Set Resolution, Scale, and Display adapter properties → Monitor → Screen refresh rate to the maximum value your monitor supports.
- **Notifications — configure & enable startup alerts** (`system-notifications` · Windows page)
  - Description: Open notification settings; enable startup app notification toggle.
  - What it does: Opens in Windows: `ms-settings:notifications`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Enable "Notify me about startup app changes" and configure app notification preferences.
- **Disable automatic Storage Sense** (`system-storage-cleanup` · registry switch)
  - Description: Turns off automatic disk cleanup to prevent unexpected file removal. Same switch as System > Storage > Storage Sense.
  - What it does: Writes the registry: `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\StorageSense\Parameters\StoragePolicy` / `01` = `0` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Enable Clipboard History (Win+V)** (`system-clipboard-history` · registry switch)
  - Description: Lets Windows store clipboard history accessible via Win+V. Same switch as System > Clipboard.
  - What it does: Writes the registry: `HKCU\SOFTWARE\Microsoft\Clipboard` / `EnableClipboardHistory` = `1` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Disable Remote Desktop (if not needed)** (`system-remote-desktop` · registry switch · administrator)
  - Description: Turns off RDP to reduce attack surface. Same switch as System > Remote Desktop.
  - What it does: Writes the registry: `HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server` / `fDenyTSConnections` = `1` (DWord). REVERT puts back the previous value from the backup.
  - Buttons: APPLY, REVERT (after this program applied it)
- **Multitasking — configure Snap windows** (`system-multitasking` · Windows page)
  - Description: Expand the Snap windows menu to configure snapping behaviour.
  - What it does: Opens in Windows: `ms-settings:multitasking`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Adjust Snap windows settings to personal preference.
- **Disable Recall AI feature (24H2+)** (`system-disable-recall` · Windows feature · administrator · restart)
  - Description: Removes the Recall optional feature via DISM. Security-conscious opt-out for 24H2+
  - What it does: Windows feature `Recall`: `Disabled`.
  - Buttons: APPLY, REVERT (after this program applied it)
- **Review optional Windows features** (`system-optional-features` · Windows page · administrator)
  - Description: Disable Windows components not needed (Hyper-V, IE mode, Print-to-PDF, etc.). Keep VBScript if you use Epic Games (installer error 2738) and Windows Media Player (legacy) for streaming to a TV.
  - What it does: Opens in Windows: `ms-settings:optionalfeatures`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Remove unused optional features. Keep only what you actively use.
- **Turn off Smart App Control** (`system-smart-app-control` · Windows page · from Max.mov's guide)
  - Description: Opens App and browser control in Windows Security. Smart App Control blocks unsigned programs; once off, turning it back on may need a Windows reset.
  - What it does: Opens in Windows: `windowsdefender://appbrowser`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Smart App Control settings - Off.
- **Guide: presentation models (how a frame reaches the screen)** (`system-presentation-model-guide` · link · from Max.mov's guide · side road)
  - Description: A detailed guide by Andrilazz: how Windows presents a frame and why it matters for VRR, latency and tearing.
  - What it does: Opens the site in your browser: `https://andrilaz.github.io/presentation-model`
  - Buttons: OPEN

#### 5.2 Maintenance

Optional. A subsection of Max.mov's guide.

- **Disable automatic Windows Maintenance** (`maintenance-disable` · registry switch · administrator · side road)
  - Description: Prevents Windows from running scheduled maintenance tasks (disk defrag, updates, diagnostics) automatically. Use with caution.
  - What it does: Writes the registry: `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\Maintenance` / `MaintenanceDisabled` = `1` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)

#### 5.3 Power Scheme

Optional.

- **How to import a power scheme** (`power-scheme-howto` · manual step · administrator · from Max.mov's guide)
  - Description: Import a .pow file then activate it. Run: powercfg -import "<path.pow>" then powercfg -setactive <GUID>.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Use the power scheme cards below: click Apply to import and activate a scheme. Click Revert to restore the previous scheme. Only one scheme should be active at a time.
- **Khorvie Power Scheme** (`power-scheme-khorvie` · power scheme · administrator · side road)
  - Description: Gaming-optimised power scheme by Khorvie. Import and set as active. Apply captures current active scheme for revert.
  - What it does: Imports and activates the power scheme `Assets\PowerSchemes\Khorvie.pow`. WINDOWS DEFAULT makes Windows' own Balanced active.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **KhorvieOS Power Scheme** (`power-scheme-khorvie-os` · power scheme · administrator · side road)
  - Description: Variant of the Khorvie scheme tuned for the KhorvieOS image.
  - What it does: Imports and activates the power scheme `Assets\PowerSchemes\KhorvieOS.pow`. WINDOWS DEFAULT makes Windows' own Balanced active.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Ultimate Performance Scheme** (`power-scheme-ultimate` · power scheme · administrator)
  - Description: Windows Ultimate Performance scheme unlocked. Maximum throughput, disables CPU parking.
  - What it does: Imports and activates the power scheme `Assets\PowerSchemes\ultimate.pow`. WINDOWS DEFAULT makes Windows' own Balanced active.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **High Performance Scheme** (`power-scheme-highperf` · power scheme · administrator)
  - Description: Standard Windows High Performance power plan.
  - What it does: Imports and activates the power scheme `Assets\PowerSchemes\high perf.pow`. WINDOWS DEFAULT makes Windows' own Balanced active.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **AdamX Power Scheme** (`power-scheme-adamx` · power scheme · administrator · side road)
  - Description: AdamX community power scheme.
  - What it does: Imports and activates the power scheme `Assets\PowerSchemes\adamx.pow`. WINDOWS DEFAULT makes Windows' own Balanced active.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Xilly Power Scheme** (`power-scheme-xilly` · power scheme · administrator · side road)
  - Description: Xilly community power scheme.
  - What it does: Imports and activates the power scheme `Assets\PowerSchemes\Xilly.pow`. WINDOWS DEFAULT makes Windows' own Balanced active.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **TJxTweaks Power Scheme** (`power-scheme-tjx` · power scheme · administrator · side road)
  - Description: TJxTweaks community power scheme.
  - What it does: Imports and activates the power scheme `Assets\PowerSchemes\TJxTweaks.pow`. WINDOWS DEFAULT makes Windows' own Balanced active.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Core Power Scheme** (`power-scheme-core` · power scheme · administrator · side road)
  - Description: Core community power scheme.
  - What it does: Imports and activates the power scheme `Assets\PowerSchemes\Core.pow`. WINDOWS DEFAULT makes Windows' own Balanced active.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Bitsium Power Scheme** (`power-scheme-bitsium` · power scheme · administrator · side road)
  - Description: Bitsium community power scheme.
  - What it does: Imports and activates the power scheme `Assets\PowerSchemes\bitsium.pow`. WINDOWS DEFAULT makes Windows' own Balanced active.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)

#### 5.4 CTT Tweaker

Optional. A subsection of Max.mov's guide.

- **CTT WinUtil — GitHub (source + releases)** (`ctt-github` · link · side road)
  - Description: Source code and releases for the Chris Titus Tech WinUtil tweaker. Run directly: irm https://github.com/ChrisTitusTech/winutil/releases/latest/download/winutil.ps1 | iex
  - What it does: Opens the site in your browser: `https://github.com/ChrisTitusTech/winutil`
  - Buttons: OPEN
- **Chris Titus Tech Win11 Tweaker** (`ctt-launch` · manual step · administrator · side road)
  - Description: All-in-one tweaker by ChrisTitusTech. Run from admin PowerShell: irm https://github.com/ChrisTitusTech/winutil/releases/latest/download/winutil.ps1 | iex
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Open an admin PowerShell and run: irm https://github.com/ChrisTitusTech/winutil/releases/latest/download/winutil.ps1 | iex — then apply the recommended tweaks: one set for desktops and laptops now (the screenshot in the guide).
- **Chris Titus Tech YouTube channel** (`ctt-channel` · link · side road)
  - Description: Reference guide and explanations for the tweaker options.
  - What it does: Opens the site in your browser: `https://www.youtube.com/@ChrisTitusTech`
  - Buttons: OPEN

#### 5.5 Devices

A subsection of Max.mov's guide.

- **Disable Bluetooth (if not needed)** (`devices-bluetooth` · Windows page)
  - Description: Turn off Bluetooth to reduce background radio usage.
  - What it does: Opens in Windows: `ms-settings:bluetooth`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Toggle Bluetooth off.
- **Disable Enhanced Pointer Precision** (`devices-pointer-precision` · Windows page)
  - Description: Turn off mouse acceleration for consistent, predictable mouse movement (important for gaming).
  - What it does: Opens in Windows: `main.cpl`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Mouse Properties open: Pointer Options tab → uncheck "Enhance pointer precision" → OK.
- **Cursor color & size** (`devices-cursor-color` · Windows page)
  - Description: Set cursor size and color scheme in accessibility settings.
  - What it does: Opens in Windows: `ms-settings:easeofaccess-mousepointer`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Choose cursor size and color (White, Black, or custom).
- **Disable Sticky Keys & Filter Keys** (`devices-sticky-keys` · Windows page)
  - Description: Disable accidental activation of Sticky Keys and Filter Keys keyboard shortcuts.
  - What it does: Opens in Windows: `ms-settings:easeofaccess-keyboard`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Turn off Sticky Keys, Filter Keys, and toggle keys.
- **Install color profile** (`devices-color-profile` · manual step)
  - Description: Apply an ICC color profile for accurate color reproduction on your monitor.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Download your monitor ICC profile from the manufacturer, from Windows Update (optional updates) or rtings.com - its reviews are paid now, older ones open through archive.org - then open Color Management (colorcpl.exe), select your display, check "Use my settings for this device" and add the profile.

#### 5.6 USB Power Management

A subsection of Max.mov's guide.

- **Disable power-saving on every device that allows it** (`usb-disable-power-mgmt` · script · administrator · side road)
  - Description: Clears "Allow the computer to turn off this device to save power" on every device that exposes that checkbox — USB hubs and controllers, network adapters, input devices, serial/USB adapters. Fixes random disconnects, at the cost of some idle power. Revert restores only the devices this card switched off.
  - What it does: A script from the manifest. Its state is checked. It has a way back.
  - Buttons: APPLY, REVERT

#### 5.7 SoundSwitch

Optional. A subsection of Max.mov's guide.

- **SoundSwitch — hotkey audio device switcher** (`soundswitch-download` · link)
  - Description: Lightweight app for switching audio output/input devices with a keyboard shortcut.
  - What it does: Opens the site in your browser: `https://github.com/Belphemur/SoundSwitch/releases`
  - Buttons: INSTALLER, PORTABLE, SITE

#### 5.8 Phone Link

Optional. A subsection of Max.mov's guide.

- **Connect Android phone to Windows (Link to Windows)** (`phone-link-guide` · manual step)
  - Description: Step-by-step guide to link your Android phone via the Phone Link app and Cross Device Experience Host.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: 1. Install "Link to Windows" on your Android phone. 2. Download Phone Link and Cross Device Exp. Host via the links below (use store.rg-adguard.net if MS Store is blocked). 3. Open Mobile Devices settings and sign in with your Microsoft account. 4. Enable shared clipboard, notifications, and other desired features in Phone Link.
- **MS Store package download (region bypass)** (`phone-link-store-bypass` · link)
  - Description: Alternative site for downloading Microsoft Store app packages (.msix) when the Store is blocked or slow.
  - What it does: Opens the site in your browser: `https://store.rg-adguard.net/`
  - Buttons: OPEN
- **Phone Link — MS Store page** (`phone-link-app` · link)
  - Description: Official Phone Link app store page. Paste the URL into store.rg-adguard.net if the Store is unavailable.
  - What it does: Opens the site in your browser: `https://apps.microsoft.com/detail/9nmpj99vjbwv`
  - Buttons: OPEN
- **Cross Device Experience Host — MS Store page** (`phone-link-cross-device-host` · link)
  - Description: Required companion component for cross-device features. Paste URL into store.rg-adguard.net if needed.
  - What it does: Opens the site in your browser: `https://apps.microsoft.com/detail/9ntxgkq8p7n0`
  - Buttons: OPEN
- **Mobile devices settings** (`phone-link-settings` · Windows page)
  - Description: Open Mobile devices settings to sign in and configure cross-device features.
  - What it does: Opens in Windows: `ms-settings:mobile-devices`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Sign in with your Microsoft account and enable shared clipboard, notifications, and other Phone Link features.

#### 5.9 Disk Indexing

Optional. A subsection of Max.mov's guide.

- **Disable Windows Search indexing service** (`indexing-disable-service` · service · administrator)
  - Description: Stops and disables the WSearch service. Reduces disk & CPU load. Start menu search still works; building an index takes longer when enabled again.
  - What it does: Service `WSearch`: startup `Disabled`, state `Stopped`. WINDOWS DEFAULT - `Automatic`.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Remove drive indexing flag (per-drive)** (`indexing-drive-properties` · manual step · administrator)
  - Description: Uncheck "Allow files on this drive to have contents indexed" in each drive's Properties.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Open File Explorer → right-click each drive → Properties → uncheck "Allow files on this drive to have contents indexed in addition to file properties".
- **Configure Search index locations** (`indexing-search-settings` · Windows page)
  - Description: Open Windows Search settings to define which folders to include in the index.
  - What it does: Opens in Windows: `ms-settings:cortana-windowssearch`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Limit indexed locations to only the folders you actually need to search.

#### 5.10 Network & Internet

A subsection of Max.mov's guide.

- **Mark Ethernet as metered connection** (`network-metered-ethernet` · Windows page)
  - Description: Prevents large background downloads (Windows Update delivery optimization, app updates) on Ethernet.
  - What it does: Opens in Windows: `ms-settings:network-ethernet`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Select your Ethernet adapter → toggle "Metered connection" On.
- **Mark Wi-Fi as metered connection** (`network-metered-wifi` · Windows page)
  - Description: Prevents large background downloads over Wi-Fi.
  - What it does: Opens in Windows: `ms-settings:network-wifi`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Select your Wi-Fi network → Properties → toggle "Metered connection" On.
- **Allow device downloads over metered connections** (`network-metered-allow-device-downloads` · Windows page · from Max.mov's guide)
  - Description: After making a connection metered, turn this on - otherwise new devices get no drivers from Windows Update.
  - What it does: Opens in Windows: `ms-settings:connecteddevices`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Devices - "Download over metered connections" - On.
- **Configure network adapter properties** (`network-adapter-properties` · Windows page · administrator)
  - Description: Open adapter settings to configure DNS servers, IPv4, and adapter-specific power settings.
  - What it does: Opens in Windows: `ms-settings:network-advancedsettings`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Set preferred DNS server to a fast provider (e.g. 1.1.1.1 / 1.0.0.1 or 8.8.8.8 / 8.8.4.4). Run DNSBench to find fastest DNS for your location.
- **DNS Benchmark — find fastest DNS for your ISP** (`network-dnsbench` · link)
  - Description: GRC DNSBench tests all known DNS resolvers and ranks them by speed from your location. The free version is on the GRC freeware page.
  - What it does: Opens the site in your browser: `https://www.grc.com/freepopular.htm`
  - Buttons: OPEN
- **Disable Cross-Device sync (if not needed)** (`network-cross-device` · Windows page)
  - Description: Turns off phone link and cross-device experience if you do not use them.
  - What it does: Opens in Windows: `ms-settings:crossdevice`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Toggle off Shared experiences and Phone Link if not needed.
- **Network settings for advanced users (Telegram)** (`network-advanced-telegram` · link · from Max.mov's guide · side road)
  - Description: The community guide to network settings in the Max.mov chat - the current version lives there.
  - What it does: Opens the site in your browser: `https://t.me/a11p1ay/6/189373`
  - Buttons: OPEN

#### 5.11 Personalization

A subsection of Max.mov's guide.

- **Wallpaper** (`personalization-wallpaper` · Windows page)
  - Description: Set desktop wallpaper from Settings or browse local images.
  - What it does: Opens in Windows: `ms-settings:personalization-background`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Choose a wallpaper image.
- **Colors & accent** (`personalization-colors` · Windows page)
  - Description: Set Dark mode and accent color.
  - What it does: Opens in Windows: `ms-settings:colors`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Choose Dark mode. Set accent color to Custom or auto from wallpaper.
- **Lock screen** (`personalization-lockscreen` · Windows page)
  - Description: Configure lock screen background and displayed info.
  - What it does: Opens in Windows: `ms-settings:lockscreen`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Set lock screen image and disable unwanted lock-screen widgets.
- **Start menu layout** (`personalization-start` · Windows page)
  - Description: Configure Start menu layout: show more pins, remove recommendations.
  - What it does: Opens in Windows: `ms-settings:personalization-start`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Set layout to "More pins". Enable "Show recently added apps" and startup notification if desired.
- **Taskbar configuration** (`personalization-taskbar` · Windows page)
  - Description: Remove taskbar widgets, search, task view buttons; enable auto-hide if desired.
  - What it does: Opens in Windows: `ms-settings:taskbar`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Disable Search, Task View, Widgets. Center or Left-align taskbar icons.
- **Disable all Device Usage suggestions** (`personalization-device-usage` · Windows page)
  - Description: Turn off Microsoft advertising and personalization features in Device Usage.
  - What it does: Opens in Windows: `ms-settings:deviceusage`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Toggle off all options in Device usage.

#### 5.12 More Personalization

Switches: 6 - the subsection head has APPLY ALL and REVERT ALL.

- **Classic right-click context menu (Win10 style)** (`personalization-classic-context-menu` · script · from Max.mov's guide · side road)
  - Description: Restores the full one-level context menu. Removes the "Show more options" extra click introduced in Win11.
  - What it does: A script from the manifest. Its state is checked. It has a way back.
  - Buttons: APPLY, REVERT
- **Remove Gallery from File Explorer navigation pane** (`personalization-remove-gallery` · registry switch · from Max.mov's guide · side road)
  - Description: Hides the Gallery entry in the Explorer left-side navigation tree.
  - What it does: Writes the registry: `HKCU\Software\Classes\CLSID\{e88865ea-0e1c-4e20-9aa6-edcd0212c87c}` / `System.IsPinnedToNameSpaceTree` = `0` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Remove Home from File Explorer navigation pane** (`personalization-remove-home` · script · side road)
  - Description: Hides the Home (MSGraph home) entry in the Explorer navigation tree.
  - What it does: A script from the manifest. Its state is checked. It has a way back.
  - Buttons: APPLY, REVERT
- **Hide Start menu Recommended section (24H2)** (`personalization-hide-start-recommendations` · script · administrator · from Max.mov's guide)
  - Description: Sets policy keys to hide the Recommended section in the Start menu. Requires 24H2 build 26100+. Uses PolicyManager keys (no GPO needed on Home). Not needed on 25H2 with the new Start: turn recommendations off in Settings - Personalization - Start.
  - What it does: A script from the manifest. Its state is checked. It has a way back.
  - Buttons: APPLY, REVERT
- **Restore Windows Photo Viewer** (`personalization-photo-viewer` · script · from Max.mov's guide · side road)
  - Description: Registers the old Photo Viewer (more correct colours than Photos) as an app images can open with, as the guide's .reg does. Then: an image → Open with → Choose another app → Windows Photo Viewer → Always.
  - What it does: A script from the manifest. Its state is checked. It has a way back.
  - Buttons: APPLY, REVERT
- **ViVeTool — download (GitHub releases)** (`download-vivetool-personalization` · link · side road)
  - Description: Tool for enabling undocumented Windows feature flags via A/B experiment IDs. Required for unlocking 25H2 and other experimental features.
  - What it does: Opens the site in your browser: `https://github.com/thebookisclosed/ViVe/releases`
  - Buttons: PORTABLE, SITE
- **Enable 25H2 feature flags (ViVeTool)** (`personalization-25h2-features` · manual step · administrator · restart · **CAREFUL** · side road)
  - Description: Unlock experimental features for Win11 25H2 using ViVeTool. This is an unofficial tool that manipulates undocumented A/B feature flags.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Codes of the guide, current on 03.05.2026 for build 26200.8328 - they change from build to build. In an admin Command Prompt in the ViVeTool folder: vivetool /enable /id:CODES, then restart; undo - vivetool /disable /id:CODES. Explorer preload 58778013; restore Explorer tabs 54572881; links in tabs of one window 49453572,49143212,48433719; new Start 47205210; Xbox full screen experience 59765208; dark theme base 49453572,48433719,58383338; dark Run (Win+R) 59270880; dark Explorer options 59203365; dark file operation dialogs 57857165,57994323. Taskbar auto-hide animation (41356296,48433719) is broken on 8328.
- **Compact (Tablet) Taskbar mode** (`personalization-compact-taskbar` · registry switch · from Max.mov's guide · side road)
  - Description: Reduces taskbar icon size and spacing via registry. Restart Explorer (or sign out) to apply.
  - What it does: Writes the registry: `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer` / `TabletPostureTaskbar` = `1` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Auto Dark/Light theme switching (PowerToys)** (`personalization-auto-dark-mode` · link · from Max.mov's guide)
  - Description: PowerToys has the Light Switch module: it switches the Windows theme on a schedule or at sunrise and sunset (it replaced the separate AutoDarkMode app).
  - What it does: Opens the site in your browser: `https://github.com/microsoft/PowerToys/releases`
  - Buttons: INSTALL, INSTALLER, SITE
- **Everything — fast file search with own index** (`personalization-everything-search` · link · from Max.mov's guide)
  - Description: Everything indexes all drive filenames and delivers sub-second search results. Download the portable version.
  - What it does: Opens the site in your browser: `https://www.voidtools.com/`
  - Buttons: INSTALLER, PORTABLE, SITE
- **Everything plugin for PowerToys Run** (`personalization-everything-powertoys-plugin` · link · from Max.mov's guide)
  - Description: Integrates Everything search into PowerToys Run (Alt+Space) for instant file lookup.
  - What it does: Opens the site in your browser: `https://github.com/lin-ycv/EverythingPowerToys/releases`
  - Buttons: INSTALLER, PORTABLE, SITE
- **DisplaySwitch — quick monitor mode shortcuts** (`personalization-display-switch` · manual step)
  - Description: Windows built-in DisplaySwitch.exe switches display mode: /1=PC screen only, /2=Duplicate, /3=Extend, /4=Second screen only.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Create desktop shortcuts to C:\Windows\System32\DisplaySwitch.exe with argument /1 (single monitor) or /3 (extend). Right-click Desktop → New → Shortcut, enter "DisplaySwitch.exe /3" as the location.

#### 5.13 Apps

A subsection of Max.mov's guide.

- **Disable background app activity** (`apps-background-apps` · Windows page)
  - Description: Prevent apps from running and updating data in the background.
  - What it does: Opens in Windows: `ms-settings:privacy-backgroundapps`
  - Buttons: OPEN IN WINDOWS
  - Instruction: For each app, disable background access or set to "Power optimized".
- **Disable transfer between devices & backup** (`apps-cross-device` · Windows page)
  - Description: Turns off cross-device transfer and app backup to Microsoft account.
  - What it does: Opens in Windows: `ms-settings:advanced-apps`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Disable "Save app state" and "Transfer to new device" options.
- **Set default apps for file extensions** (`apps-default-file-associations` · Windows page)
  - Description: Assign preferred apps for file types (e.g. browser for HTML, image viewer for PNG).
  - What it does: Opens in Windows: `ms-settings:defaultapps`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Set default application for each file extension you use regularly.
- **Disable unnecessary startup apps** (`apps-startup` · Windows page)
  - Description: Review and disable apps that launch at Windows startup.
  - What it does: Opens in Windows: `ms-settings:startupapps`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Toggle off any startup apps you do not actively use.
- **Install all Microsoft Store app updates** (`apps-store-update` · Windows page)
  - Description: Ensure all Store apps are up-to-date before configuring defaults.
  - What it does: Opens in Windows: `ms-windows-store://downloadsandupdates/`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Click "Get updates" and wait for all apps to update.
- **Game Bar: keep it on a Ryzen with two CCDs** (`apps-gamebar-note` · manual step · from Max.mov's guide · side road)
  - Description: Game Bar tells Windows which apps are games, so they run on the right CCD of a dual-CCD Ryzen (7950X3D and alike). It also has "Remember this is a game" and a handy volume mixer.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Dual-CCD Ryzen: do not remove Game Bar; for a game Windows does not recognise, press Win+G and tick "Remember this is a game". Removing it elsewhere: after an uninstaller, the ms-gamebar links still pop up windows when games start - the card below silences them.
- **After removing Game Bar: silence its leftovers** (`apps-gamebar-leftovers` · script · **CAREFUL** · from Max.mov's guide · side road)
  - Description: Once Game Bar is uninstalled, games still call the ms-gamebar links and Windows pops up "get an app" windows. This points both links to a silent system program and turns Game DVR capture off. Not on a Ryzen with two CCDs.
  - What it does: A script from the manifest. Its state is checked. It has a way back.
  - Buttons: APPLY, REVERT

#### 5.14 Other Startup Settings

Optional. A subsection of Max.mov's guide.

- **Open User Startup folder** (`startup-user-folder` · script)
  - Description: Opens the current-user Startup folder in Explorer. Shortcuts here launch at login for this user only.
  - What it does: A script from the manifest. No state check: the button just runs it.
  - Buttons: RUN
- **Open System Startup folder** (`startup-system-folder` · script · administrator)
  - Description: Opens the system-wide Startup folder in Explorer. Shortcuts here launch at login for all users.
  - What it does: A script from the manifest. No state check: the button just runs it.
  - Buttons: RUN
- **Sysinternals Autoruns — comprehensive startup manager** (`startup-autoruns` · link)
  - Description: Shows every auto-start location: registry, drivers, scheduled tasks, services, browser extensions, and more.
  - What it does: Opens the site in your browser: `https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns`
  - Buttons: PORTABLE, SITE

#### 5.15 Language & Time

A subsection of Max.mov's guide.

- **Clock on taskbar — format & display** (`language-taskbar-clock` · Windows page)
  - Description: Open Date and time settings: the clock of the system tray and additional calendars.
  - What it does: Opens in Windows: `ms-settings:dateandtime`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Show time and date in the system tray, and seconds in the clock if desired.
- **Disable unnecessary input features** (`language-input-settings` · Windows page)
  - Description: Remove hardware keyboard suggestions, autocorrect, and other input method features.
  - What it does: Opens in Windows: `ms-settings:typing`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Turn off Autocorrect, Spell check, and Text predictions.
- **Language keyboard shortcut** (`language-keyboard-shortcut` · Windows page)
  - Description: Configure the hotkey for switching input languages.
  - What it does: Opens in Windows: `ms-settings:keyboard`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Click "Advanced keyboard settings" → Input language hotkeys → set to preferred shortcut or "Not assigned".
- **Open Input Language Hotkeys dialog** (`language-open-hotkey-dialog` · script)
  - Description: Opens the classic Advanced Key Settings dialog where you can view and change the keyboard language switch shortcut.
  - What it does: A script from the manifest. No state check: the button just runs it.
  - Buttons: RUN
- **Switch language hotkey: Alt+Shift → Ctrl+Shift** (`language-keyboard-toggle-ctrlshift` · script · side road)
  - Description: Changes the keyboard language switch shortcut from Alt+Shift (Windows default) to Ctrl+Shift. Takes effect after sign-out or Explorer restart.
  - What it does: A script from the manifest. Its state is checked. It has a way back.
  - Buttons: APPLY, REVERT

#### 5.16 Privacy

A subsection of Max.mov's guide. Switches: 2 - the subsection head has APPLY ALL and REVERT ALL.

- **Disable all General privacy options** (`privacy-general` · Windows page)
  - Description: Turns off advertising ID, language list access, app launch tracking, suggested content, and settings page recommendations.
  - What it does: Opens in Windows: `ms-settings:privacy-general`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Toggle off every option on this page.
- **Disable online speech recognition** (`privacy-speech` · registry switch)
  - Description: Prevents Microsoft from collecting voice data for speech model improvement. Same switch as Privacy & security > Speech.
  - What it does: Writes the registry: `HKCU\SOFTWARE\Microsoft\Speech_OneCore\Settings\OnlineSpeechPrivacy` / `HasAccepted` = `0` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Disable inking & typing personalization** (`privacy-inking` · Windows page)
  - Description: Stops Microsoft from collecting typed and handwritten data for custom dictionary.
  - What it does: Opens in Windows: `ms-settings:privacy-speechtyping`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Turn off "Getting to know you" personalization.
- **Set diagnostics to Required only, delete data** (`privacy-diagnostics` · Windows page · administrator)
  - Description: Reduce telemetry to the minimum allowed. Delete previously collected diagnostic data.
  - What it does: Opens in Windows: `ms-settings:privacy-feedback`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Set "Diagnostic data" to Required only. Click "Delete diagnostic data" to remove existing data. Set feedback frequency to Never.
- **Disable all Search permissions** (`privacy-search-permissions` · Windows page)
  - Description: Prevents Windows Search from sending search history and web suggestions to Microsoft.
  - What it does: Opens in Windows: `ms-settings:search-permissions`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Toggle off all options: SafeSearch, Cloud content search, Search history, Better search suggestions.
- **Location services** (`privacy-location` · Windows page)
  - Description: Opens the Location page of Windows Settings: the system-wide switch and the per-app permissions. Set it there - Windows keeps this switch in more than one place.
  - What it does: Opens in Windows: `ms-settings:privacy-location`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Turn Location services off (or on) with the switch at the top of the page.
- **Disable app diagnostic data access** (`privacy-app-diagnostics` · registry switch · administrator)
  - Description: Prevents apps from reading other apps diagnostic information. Same switch as Privacy & security > App diagnostics.
  - What it does: Writes the registry: `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\CapabilityAccessManager\ConsentStore\appDiagnostics` / `Value` = `Deny` (String). REVERT puts back the previous value from the backup. WINDOWS DEFAULT = `Allow`.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Disable custom device data sharing** (`privacy-custom-devices` · Windows page)
  - Description: Prevents apps from sharing data with custom devices on the network.
  - What it does: Opens in Windows: `ms-settings:privacy-customdevices`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Toggle off "Communicate with unpaired devices".

#### 5.17 Windows Update

A subsection of Max.mov's guide. Switches: 2 - the subsection head has APPLY ALL and REVERT ALL.

- **Check for Windows Updates** (`updates-check` · Windows page)
  - Description: Verify all Windows updates are installed before final configuration.
  - What it does: Opens in Windows: `ms-settings:windowsupdate`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Click "Check for updates" and install all available updates. Restart if prompted.
- **Check optional & driver updates** (`updates-optional` · Windows page)
  - Description: Review optional updates for driver updates not delivered via main Windows Update.
  - What it does: Opens in Windows: `ms-settings:windowsupdate-optionalupdates`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Install optional driver updates if relevant to your hardware.
- **Disable Delivery Optimization (P2P updates)** (`updates-delivery-optimization` · registry switch · administrator)
  - Description: Prevents Windows from using your internet connection to send updates to other PCs. Writes the policy value, which also greys the switch out in Settings.
  - What it does: Writes the registry: `HKLM\SOFTWARE\Policies\Microsoft\Windows\DeliveryOptimization` / `DODownloadMode` = `0` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Disable Find My Device (if not needed)** (`updates-find-my-device` · registry switch · administrator)
  - Description: Turns off device location tracking via Microsoft account. Same switch as Privacy & security > Find my device.
  - What it does: Writes the registry: `HKLM\SOFTWARE\Microsoft\PolicyManager\default\Experience\AllowFindMyDevice` / `value` = `0` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT = `1`.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)

### 6. GPU & Monitor Settings

#### 6.0 Important Notes

A subsection of Max.mov's guide.

- **v0.4 update notes — VRR and Nvidia settings** (`gpu-notes-v04` · manual step)
  - Description: Key corrections from v0.4: G-Sync should be enabled in fullscreen mode only (not windowed+fullscreen). Nvidia settings section now includes DLSS model/preset selection and PS-native DLSS indicator toggle.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: VRR monitors: use "Enable G-Sync for full screen mode" only — do NOT enable for windowed+fullscreen mode. See the VRR guide link below for details.

#### 6.1 Monitor Setup

A subsection of Max.mov's guide.

- **Monitor setup guide — fixed refresh rate (no VRR)** (`monitor-no-vrr-guide` · link · side road)
  - Description: Online guide for configuring a monitor that does not support Variable Refresh Rate (G-Sync / FreeSync).
  - What it does: Opens the site in your browser: `https://andrilaz.github.io/fixed-refresh`
  - Buttons: OPEN
- **Monitor setup guide — VRR (G-Sync / FreeSync)** (`monitor-vrr-guide` · link · side road)
  - Description: Online guide for configuring a VRR monitor with G-Sync or FreeSync. Includes correct G-Sync mode selection.
  - What it does: Opens the site in your browser: `https://andrilaz.github.io/vrr`
  - Buttons: OPEN

#### 6.2 Nvidia Settings

A subsection of Max.mov's guide. Switches: 2 - the subsection head has APPLY ALL and REVERT ALL.

- **Open Nvidia Control Panel** (`nvidia-control-panel` · Windows page)
  - Description: Open the Nvidia Control Panel from the Microsoft Store to access display, 3D settings, and G-Sync configuration.
  - What it does: Opens in Windows: `ms-windows-store://pdp/?ProductId=9NF8H0H7WMLT`
  - Buttons: OPEN IN WINDOWS
  - Instruction: Install or open the Nvidia Control Panel from the Store, then configure 3D Settings and Display settings per the guide.
- **Configure Nvidia driver settings (3D, DLSS, display)** (`nvidia-settings-guide` · manual step · side road)
  - Description: Apply recommended Nvidia Control Panel settings: power management mode, texture filtering, DLSS model/preset selection, and display configuration.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In Nvidia Control Panel: 1. Manage 3D settings → Power management mode: Prefer maximum performance. 2. Set texture filtering quality. 3. G-Sync: always "Enable for full screen mode" - never "windowed and full screen mode" (the video had it wrong). 4. DLSS model and preset: in Nvidia Profile Inspector - see the DLSS card below. Refer to the Nvidia settings guide in the archive for detailed recommendations.
- **Download Nvidia Profile Inspector** (`download-nvidia-profile-inspector` · link · from Max.mov's guide · side road)
  - Description: NVPI edits the driver profile settings the Control Panel does not show: the DLSS override, the preset letter, Ansel and more.
  - What it does: Opens the site in your browser: `https://github.com/Orbmu2k/nvidiaProfileInspector/releases`
  - Buttons: PORTABLE, SITE
- **DLSS: choose the model and preset (NVPI)** (`nvidia-dlss-models-presets` · manual step · administrator · from Max.mov's guide · side road)
  - Description: The latest DLSS version in every game and the model that suits your card - set once in Nvidia Profile Inspector.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In Nvidia Profile Inspector, global profile: 1. "DLSS - Enable DLSS Override" - "On - DLSS Overridden by Latest Version". 2. "Forced Preset Letter" - the model: M or L (DLSS 4.5) for Performance and Ultra Performance on RTX 40/50; K or J (Transformer) for any quality level; E or F (CNN) for weaker RTX 20/30; A-D are outdated. 3. "Forced Quality Level" - if one level everywhere is wanted. 4. Sharpness - "Sharpening Filter". Apply changes. Which model a game really uses shows the DLSS indicator (card below).
- **Turn off Ansel (the Nvidia App filters go too)** (`nvidia-disable-ansel` · manual step · administrator · from Max.mov's guide · side road)
  - Description: Ansel (NvCamera) is the capture and filter layer of the driver. Turning it off also removes the filters of the Nvidia App overlay.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In Nvidia Profile Inspector, global profile: "Ansel Enabled" - Off, apply. The filters of the Nvidia App overlay become unavailable: if the filter list shows only "RTX Dynamic Vibrance", Ansel is off. To get the filters back, set "Ansel Enabled" to On again.
- **Show Nvidia DLSS indicator overlay** (`nvidia-dlss-indicator` · registry switch · administrator · restart)
  - Description: Enables the Nvidia NGXCore DLSS indicator overlay via the driver registry flag.
  - What it does: Writes the registry: `HKLM\SOFTWARE\NVIDIA Corporation\Global\NGXCore` / `ShowDlssIndicator` = `1` (DWord). REVERT puts back the previous value from the backup. WINDOWS DEFAULT removes the value.
  - Buttons: APPLY, REVERT (after this program applied it), WINDOWS DEFAULT (when the value is not the default)
- **Disable Nvidia HDCP** (`nvidia-disable-hdcp` · script · administrator · restart · **CAREFUL** · side road)
  - Description: Sets the Nvidia display driver HDCP override flag on detected Nvidia display class registry entries. Revert removes the override flag.
  - What it does: A script from the manifest. Its state is checked. It has a way back.
  - Buttons: APPLY, REVERT

#### 6.3 Official Nvidia Recommendations

- **NVIDIA App — download** (`nvidia-app-download` · link)
  - Description: Official NVIDIA App download page for drivers, game optimization, DLSS Overrides, overlays, GPU tuning, and RTX video features.
  - What it does: Opens the site in your browser: `https://www.nvidia.com/en-us/software/nvidia-app/`
  - Buttons: INSTALLER, SITE
- **Apply NVIDIA App optimal game settings** (`nvidia-app-optimal-settings` · manual step)
  - Description: Use NVIDIA App recommendations for supported games and apps. Recommendations are based on GPU, CPU, resolution, RAM, OS, and the latest official game patch.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Open NVIDIA App → Home or Graphics → select a detected game → Optimize. Run a game once if NVIDIA App cannot optimize it yet, update the game and driver, then use the Performance/Quality slider if needed.
- **Configure NVIDIA App DLSS Overrides** (`nvidia-app-dlss-overrides` · manual step)
  - Description: Use official DLSS Overrides to apply newer DLSS models, DLSS Super Resolution presets, Frame Generation options, DLAA, and Ultra Performance modes globally or per game.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Open NVIDIA App → Graphics → Global Settings or Program Settings → DLSS Override. For Super Resolution, choose Model Presets → Recommended. For Frame Generation, choose Dynamic or Fixed only for compatible RTX GPUs/games. Use Statistics → DLSS in the NVIDIA overlay to verify override status.
- **Official G-SYNC / VRR and V-Sync baseline** (`nvidia-gsync-vsync-baseline` · manual step · from Max.mov's guide)
  - Description: Configure VRR the NVIDIA-supported way: enable display VRR, use the maximum refresh rate, enable G-SYNC for full screen mode, and avoid forcing V-Sync Off globally.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Enable Adaptive Sync / G-SYNC in the monitor OSD, set the highest supported refresh rate in Windows or NVIDIA Control Panel, then NVIDIA Control Panel → Set up G-SYNC → enable for Full screen mode. For no-tear G-SYNC use, set Manage 3D Settings → Vertical Sync → On; do not force global V-Sync Off.
- **NVIDIA Reflex and Ultra Low Latency** (`nvidia-reflex-low-latency` · manual step · from Max.mov's guide)
  - Description: Use NVIDIA Reflex in supported games; use Ultra Low Latency Mode in the driver when Reflex is unavailable.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: In supported games, enable NVIDIA Reflex Low Latency (On, or On + Boost if needed). If a game does not support Reflex, open NVIDIA Control Panel → Manage 3D Settings → Low Latency Mode → Ultra, preferably per game.
- **Max Frame Rate and Power Management** (`nvidia-max-frame-rate-power` · manual step · from Max.mov's guide)
  - Description: Use NVIDIA Max Frame Rate for latency, power saving, or staying inside the VRR range; use Prefer Maximum Performance only when needed.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: NVIDIA Control Panel → Manage 3D Settings → Max Frame Rate. For VRR, cap slightly below the display maximum refresh rate. For latency in GPU-bound games, use Prefer maximum performance and Low Latency Mode Ultra. For power saving, use Optimal Power.
- **NVIDIA Image Scaling** (`nvidia-image-scaling` · manual step · restart · from Max.mov's guide)
  - Description: Enable driver-level spatial upscaling and sharpening for supported DirectX, Vulkan, and OpenGL games.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: NVIDIA Control Panel → Manage 3D Settings → Image Scaling → On. Reboot so games detect the generated scaling resolutions, then choose a lower render resolution in fullscreen mode and tune sharpening globally or per game.
- **RTX Video Super Resolution / HDR** (`nvidia-rtx-video` · manual step)
  - Description: Enable RTX Video enhancements through NVIDIA App or NVIDIA Control Panel for supported browsers and RTX GPUs.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Prefer NVIDIA App for RTX Video options. Alternatively open NVIDIA Control Panel → Adjust Video Image Settings → RTX Video Enhancements, then enable Super Resolution and/or HDR. Use a supported browser such as Chrome, Edge, or Firefox.
- **NVIDIA performance and DLSS status overlay** (`nvidia-performance-overlay` · manual step)
  - Description: Use the NVIDIA overlay to show FPS, GPU metrics, latency metrics, and DLSS Override status in-game.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Enable In-Game Overlay in NVIDIA App. Press Alt+Z → Statistics → choose DLSS or Custom statistics view. Toggle the overlay with Alt+R while in game.

### 7. Cooling Setup

#### 7.0 Cooling Setup

A subsection of Max.mov's guide.

- **Enable PWM (Smart) fan mode in BIOS** (`enable-pwm-bios` · manual step · restart)
  - Description: Set fan headers to PWM (Smart) mode in BIOS/UEFI so software like Fan Control can regulate fan speeds. Only applicable if your motherboard and fans support PWM.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Restart into BIOS/UEFI (usually Del or F2 on boot). Navigate to the fan/hardware monitoring section. Set each fan header connected to a PWM fan to "PWM" or "Smart" mode. Save and exit.
- **Download Fan Control** (`download-fan-control` · link)
  - Description: Fan Control is a free open-source app for detailed fan curve configuration, temperature monitoring, and mixing sensor inputs.
  - What it does: Opens the site in your browser: `https://github.com/Rem0o/FanControl.Releases/releases`
  - Buttons: INSTALLER, PORTABLE, SITE

### 8. Steam & Game Launchers

#### 8.0 Game Launchers

- **Download Steam** (`download-steam` · link · from Max.mov's guide)
  - Description: Official Steam client download page.
  - What it does: Opens the site in your browser: `https://store.steampowered.com/about/`
  - Buttons: INSTALL, INSTALLER, SITE
- **Download Epic Games Store** (`download-epic-games` · link)
  - Description: Official Epic Games Store launcher download page.
  - What it does: Opens the site in your browser: `https://store.epicgames.com/download`
  - Buttons: INSTALL, INSTALLER, SITE
- **Download EA App** (`download-ea-app` · link · from Max.mov's guide)
  - Description: Official EA App launcher download page (replaces Origin).
  - What it does: Opens the site in your browser: `https://www.ea.com/ea-app`
  - Buttons: INSTALLER, SITE
- **Install EA App to a custom drive** (`ea-install-other-drive` · manual step · administrator · from Max.mov's guide)
  - Description: The EA installer supports a command-line argument to set the default install folder. Use this to install games on a secondary drive.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Run the EA installer from the command line with: EAappInstaller.exe /i DefaultInstallFolder="D:\Gaming\EA Games" — replace D:\Gaming\EA Games with your preferred path.
- **Download Blizzard Battle.net** (`download-blizzard` · link)
  - Description: Official Battle.net desktop app download page.
  - What it does: Opens the site in your browser: `https://download.battle.net/desktop`
  - Buttons: INSTALLER, SITE
- **Download Rockstar Games Launcher** (`download-rockstar` · link)
  - Description: Official Rockstar Games Launcher download page (required for GTA V, RDR2, etc.).
  - What it does: Opens the site in your browser: `https://socialclub.rockstargames.com/rockstar-games-launcher`
  - Buttons: INSTALLER, SITE
- **Download Xbox app** (`download-xbox` · link)
  - Description: Official Xbox app for PC from the Microsoft Store. Provides access to Game Pass and Xbox games.
  - What it does: Opens the site in your browser: `https://www.microsoft.com/store/productId/9MV0B5HZVK9Z`
  - Buttons: OPEN
- **Download Valorant / League of Legends** (`download-valorant-lol` · link)
  - Description: Riot Games download page for Valorant and League of Legends (includes the Riot Client).
  - What it does: Opens the site in your browser: `https://playvalorant.com/download/`
  - Buttons: INSTALLER, SITE
- **Download TcNo Account Switcher** (`download-account-switcher` · link · from Max.mov's guide · side road)
  - Description: Convenient multi-account switcher for Steam, Epic Games, EA, and other gaming platforms.
  - What it does: Opens the site in your browser: `https://github.com/TCNOco/TcNo-Acc-Switcher/releases`
  - Buttons: INSTALLER, PORTABLE, SITE

#### 8.1 Minecraft

A subsection of Max.mov's guide.

- **Minecraft launcher notes (v0.4)** (`minecraft-notes` · manual step)
  - Description: Correction from v0.4: Prism Launcher does NOT support playing without a license. Use Freesm Launcher (fork of Prism) for playing without a license.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Use Freesm Launcher (link 3.1) if you want to play without a license. Prism Launcher requires a valid Minecraft license.
- **Download Minecraft (Bedrock Edition — official, requires license)** (`minecraft-bedrock` · link)
  - Description: Official Minecraft Bedrock Edition from the Microsoft Store.
  - What it does: Opens the site in your browser: `https://www.microsoft.com/store/productId/9NBLGGH2JHXJ`
  - Buttons: OPEN
- **Download Minecraft Preview (Bedrock early access — official, requires license)** (`minecraft-bedrock-preview` · link)
  - Description: Early access builds of Minecraft Bedrock Edition from the Microsoft Store.
  - What it does: Opens the site in your browser: `https://www.microsoft.com/store/productId/9P5X4QVLC2XR`
  - Buttons: OPEN
- **Download Minecraft Launcher (Bedrock + Java — official, requires license)** (`minecraft-launcher-unified` · link)
  - Description: Unified Minecraft Launcher supporting both Bedrock and Java editions, from the Microsoft Store.
  - What it does: Opens the site in your browser: `https://www.microsoft.com/store/productId/9PGW18NPBZV5`
  - Buttons: OPEN
- **Download Minecraft Launcher without Microsoft Store (Java Edition — official)** (`minecraft-launcher-no-store` · link)
  - Description: Direct download for the Minecraft Java Edition launcher without using the Microsoft Store.
  - What it does: Opens the site in your browser: `https://www.minecraft.net/download`
  - Buttons: INSTALL, INSTALLER, SITE
- **Download Prism Launcher (unofficial, requires license)** (`minecraft-prism-launcher` · link · side road)
  - Description: Open-source Minecraft launcher with modpack management. Requires a valid Minecraft license.
  - What it does: Opens the site in your browser: `https://github.com/PrismLauncher/PrismLauncher/releases`
  - Buttons: INSTALLER, PORTABLE, SITE
- **Download Freesm Launcher (unofficial, no license required)** (`minecraft-freesm-launcher` · link · side road)
  - Description: Fork of Prism Launcher that supports playing without a Minecraft license.
  - What it does: Opens the site in your browser: `https://github.com/FreesmTeam/FreesmLauncher/releases`
  - Buttons: INSTALLER, PORTABLE, SITE
- **Download MultiMC Launcher (outdated, unofficial, requires license)** (`minecraft-multimc` · link · side road)
  - Description: Legacy MultiMC launcher — outdated, superseded by Prism. Requires a valid Minecraft license.
  - What it does: Opens the site in your browser: `https://github.com/MultiMC/Launcher/releases`
  - Buttons: OPEN
- **Browse Java Edition mods on Modrinth** (`minecraft-mods-modrinth` · link)
  - Description: Modrinth mod repository for Minecraft Java Edition mods and modpacks.
  - What it does: Opens the site in your browser: `https://modrinth.com/mods`
  - Buttons: OPEN
- **Browse Java Edition mods on CurseForge** (`minecraft-mods-curseforge` · link)
  - Description: CurseForge mod repository for Minecraft Java Edition mods and modpacks.
  - What it does: Opens the site in your browser: `https://www.curseforge.com/minecraft`
  - Buttons: OPEN
- **Download Modrinth app (mod manager)** (`minecraft-modrinth-app` · link)
  - Description: Modrinth desktop app for convenient mod and modpack installation. Requires a Minecraft license.
  - What it does: Opens the site in your browser: `https://modrinth.com/app`
  - Buttons: INSTALLER, SITE
- **Download CurseForge app (mod manager)** (`minecraft-curseforge-app` · link)
  - Description: CurseForge desktop app for convenient mod and modpack installation.
  - What it does: Opens the site in your browser: `https://www.curseforge.com/download/app`
  - Buttons: INSTALLER, SITE
- **Download Minecraft server (Java or Bedrock — official)** (`minecraft-server` · link)
  - Description: Official Minecraft server software download page for both Java and Bedrock editions.
  - What it does: Opens the site in your browser: `https://www.minecraft.net/download`
  - Buttons: OPEN

### 9. Global Timer Resolution

#### 9.0 Global Timer Resolution

A subsection of Max.mov's guide.

- **Timer Resolution — important notes (v0.3)** (`timer-resolution-notes` · manual step · administrator · side road)
  - Description: The old Timer Resolution executable archive is intentionally not shipped in this project. Treat donor material as reference only; source and review any tools separately if you deliberately want to test them.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: There is no local Timer Resolution archive to extract or run. If you obtain timer-resolution tools separately, verify the source/signature, scan the files, and test only if you understand the power, latency, and stability trade-offs. Most users will not need this tweak.
- **What is Global Timer Resolution?** (`timer-resolution-what-it-does` · manual step · administrator · side road)
  - Description: Windows uses a system-wide timer interrupt that defaults to 15.625 ms. Setting a higher resolution (e.g. 0.5 ms) can reduce micro-stutter in games, but it increases CPU power consumption and may destabilise some workloads. Not recommended for most users.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Use this card as background context only. This project does not install or launch Timer Resolution. Only apply a separately sourced utility if you understand the trade-offs; revert by stopping the utility or uninstalling it.

### 10. FPS & Latency Testing

#### 10.0 Testing Notes

A subsection of Max.mov's guide.

- **v0.4 testing notes — Intel PresentMon & PCLatency** (`testing-notes-v04` · manual step)
  - Description: Download the .msi installer for the GUI version of Intel PresentMon (as shown in the video), or the x64 .exe for the lightweight console version. PCLatency must now be downloaded manually from GitHub — if flagged by antivirus, add it to Windows Defender exclusions.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Intel PresentMon: download .msi for the overlay GUI, or x64 .exe for the console version. Console version is lighter and more accurate — display via a second monitor or PowerToys "Always on top". Note: the console version always creates .csv files; delete them when not needed. PCLatency: download from GitHub; add to Defender exclusions if flagged.

#### 10.1 Testing Tools

A subsection of Max.mov's guide.

- **Download CapFrameX** (`download-capframex` · link)
  - Description: CapFrameX is a frame time capture and analysis tool with an in-game overlay. Records and visualises frametimes, FPS statistics, and GPU/CPU sensor data.
  - What it does: Opens the site in your browser: `https://github.com/CXWorld/CapFrameX/releases`
  - Buttons: INSTALLER, PORTABLE, SITE
- **Download Intel PresentMon** (`download-intel-presentmon` · link)
  - Description: Intel PresentMon measures frame presentation latency and FPS using ETW. Available as a GUI overlay (.msi) or lightweight console tool (x64 .exe).
  - What it does: Opens the site in your browser: `https://github.com/GameTechDev/PresentMon/releases`
  - Buttons: INSTALL, INSTALLER, PORTABLE, SITE
- **Download Nvidia FrameView** (`download-nvidia-frameview` · link)
  - Description: Nvidia FrameView captures frame times, FPS, and power data. Works with both Nvidia and AMD GPUs.
  - What it does: Opens the site in your browser: `https://www.nvidia.com/en-us/geforce/technologies/frameview/`
  - Buttons: OPEN
- **Download PCLatency** (`download-pclatency` · link · side road)
  - Description: PCLatency measures PC system latency and visualises results from CapFrameX .csv files. May be flagged as false positive by antivirus — add to Defender exclusions if needed.
  - What it does: Opens the site in your browser: `https://github.com/notch4ff4/pc-latency-view/releases`
  - Buttons: PORTABLE, SITE
- **Nvidia article — Understanding and measuring PC latency** (`nvidia-latency-article` · link)
  - Description: Official Nvidia developer article explaining end-to-end PC latency, how to measure it, and what affects it.
  - What it does: Opens the site in your browser: `https://developer.nvidia.com/blog/understanding-and-measuring-pc-latency/`
  - Buttons: OPEN

### 11. Recommended Programs

#### 11.0 Community Resources

- **Viewer-recommended software (Telegram chat topic)** (`telegram-viewer-software` · link · side road)
  - Description: Telegram topic where viewers share their own recommended software picks.
  - What it does: Opens the site in your browser: `https://t.me/a11p1ay`
  - Buttons: OPEN

#### 11.1 Browsers

- **Google Chrome** (`download-chrome` · link)
  - Description: The most widely used browser. Best site compatibility, V8 engine, sync across devices.
  - What it does: Opens the site in your browser: `https://www.google.com/chrome/`
  - Buttons: INSTALL, INSTALLER, SITE
- **Brave Browser** (`download-brave` · link)
  - Description: Chromium-based with built-in ad and tracker blocking. No Google telemetry. Good privacy defaults.
  - What it does: Opens the site in your browser: `https://brave.com/`
  - Buttons: INSTALLER, SITE
- **Mozilla Firefox** (`download-firefox` · link)
  - Description: Independent Gecko engine, not Chromium. Strong privacy defaults and excellent extension ecosystem.
  - What it does: Opens the site in your browser: `https://www.mozilla.org/firefox/`
  - Buttons: INSTALL, INSTALLER, SITE
- **Vivaldi** (`download-vivaldi` · link)
  - Description: Chromium-based. Extreme UI customisation, built-in tab groups, notes, and mail client. For power users.
  - What it does: Opens the site in your browser: `https://vivaldi.com/`
  - Buttons: INSTALLER, SITE
- **Zen Browser** (`download-zen-browser` · link)
  - Description: Firefox-based. Privacy-focused with a clean UI and tab workspaces. Actively developed.
  - What it does: Opens the site in your browser: `https://zen-browser.app/`
  - Buttons: INSTALLER, PORTABLE, SITE

#### 11.2 System Utilities

- **Download Microsoft PowerToys** (`download-powertoys` · link · from Max.mov's guide)
  - Description: PowerToys adds useful Windows utilities: PowerToys Run, FancyZones, Keyboard Manager, Color Picker, Image Resizer, Light Switch (automatic dark and light theme), and more.
  - What it does: Opens the site in your browser: `https://github.com/microsoft/PowerToys/releases`
  - Buttons: INSTALL, INSTALLER, SITE
- **Download Everything (fast file search)** (`download-everything` · link · from Max.mov's guide)
  - Description: Everything indexes all file names on your drives instantly. Sub-second search results across all drives.
  - What it does: Opens the site in your browser: `https://www.voidtools.com/`
  - Buttons: INSTALLER, PORTABLE, SITE
- **Download ViVeTool (unlock hidden Windows features)** (`download-vivetool` · link · from Max.mov's guide · side road)
  - Description: ViVeTool activates hidden A/B feature flags in Windows. Useful for enabling experimental features before they roll out to your account. Feature IDs are build-specific and change over time. Setup: unpack into Apps, open Terminal in that folder, make Command Prompt the default profile and run it as administrator.
  - What it does: Opens the site in your browser: `https://github.com/thebookisclosed/ViVe/releases`
  - Buttons: PORTABLE, SITE
- **Microsoft PC Manager** (`download-pc-manager` · link)
  - Description: Official Microsoft utility for RAM cleanup, temp file removal, and system health overview. Opens directly in the Microsoft Store.
  - What it does: Opens the site in your browser: `https://apps.microsoft.com/detail/9pm860492szd`
  - Buttons: OPEN
- **Microsoft PC Manager — direct .msix download (if Store unavailable)** (`pc-manager-msix-bypass` · link · side road)
  - Description: Use store.rg-adguard.net to get the direct .msix package link. Paste the Store URL into the search box to obtain the download link.
  - What it does: Opens the site in your browser: `https://store.rg-adguard.net/`
  - Buttons: OPEN
- **Download Ventoy (bootable USB creator)** (`download-ventoy` · link)
  - Description: Ventoy creates a multiboot USB drive — just copy ISO files onto it, no re-flashing needed.
  - What it does: Opens the site in your browser: `https://github.com/ventoy/Ventoy`
  - Buttons: PORTABLE, SITE
- **Download PathScan (path length viewer)** (`download-pathscan` · link · side road)
  - Description: PathScan displays folder and file path lengths, helping identify paths that exceed the Windows 260-character limit.
  - What it does: Opens the site in your browser: `https://www.softpedia.com/get/System/File-Management/Path-Scan.shtml#download`
  - Buttons: OPEN

#### 11.3 File Management

- **Download ExplorerTabUtility (improved Explorer tabs)** (`download-explorer-tab-utility` · link · from Max.mov's guide · side road)
  - Description: ExplorerTabUtility forces all Explorer windows to open as tabs instead of new windows. Adds browser-like tab shortcuts (Ctrl+D, Ctrl+Shift+T). On Windows 25H2+, the built-in option "open folders in new tab" already covers most cases, but ETU still adds hotkey support.
  - What it does: Opens the site in your browser: `https://github.com/w4po/ExplorerTabUtility/releases`
  - Buttons: INSTALLER, PORTABLE, SITE
- **Download Symbolic11 (symlink manager)** (`download-symbolic11` · link · side road)
  - Description: GUI tool for conveniently creating and managing symbolic links on Windows.
  - What it does: Opens the site in your browser: `https://github.com/Benisgo/Symbolic11`
  - Buttons: PORTABLE, SITE
- **Download Nilesoft Shell (custom context menu)** (`download-nilesoft-shell` · link · side road)
  - Description: Nilesoft Shell lets you build a fully custom right-click context menu, replacing the default Windows 11 context menu.
  - What it does: Opens the site in your browser: `https://nilesoft.org/download`
  - Buttons: INSTALL, INSTALLER, SITE

#### 11.4 Uninstallers

- **Download Bulk Crap Uninstaller (BCUninstaller)** (`download-bulk-crap-uninstaller` · link)
  - Description: Open-source batch uninstaller that removes apps including leftovers, supports silent uninstall, and lists Store/Chocolatey/Scoop packages.
  - What it does: Opens the site in your browser: `https://github.com/Klocman/Bulk-Crap-Uninstaller`
  - Buttons: INSTALLER, PORTABLE, SITE
- **Download Revo Uninstaller Free** (`download-revo-uninstaller` · link · from Max.mov's guide · side road)
  - Description: Revo Uninstaller removes programs and then scans for leftover registry entries and files.
  - What it does: Opens the site in your browser: `https://www.revouninstaller.com/products/revo-uninstaller-free/`
  - Buttons: INSTALL, INSTALLER, SITE

#### 11.5 Security & Privacy

- **Download Microsoft Safety Scanner (on-demand AV scan)** (`download-microsoft-safety-scanner` · link · from Max.mov's guide)
  - Description: Free on-demand malware scanner from Microsoft. Does not replace a real-time antivirus — use for one-time scanning.
  - What it does: Opens the site in your browser: `https://learn.microsoft.com/en-us/defender-endpoint/safety-scanner-download`
  - Buttons: PORTABLE, SITE
- **Download Kaspersky Virus Removal Tool** (`download-kaspersky-vrt` · link · from Max.mov's guide)
  - Description: Free standalone virus removal tool from Kaspersky. Useful for scanning a system you suspect is infected.
  - What it does: Opens the site in your browser: `https://www.kaspersky.ru/downloads/free-virus-removal-tool`
  - Buttons: OPEN
- **VirusTotal — online file & URL scanner** (`virustotal` · link)
  - Description: Scan any file or URL against 70+ antivirus engines simultaneously.
  - What it does: Opens the site in your browser: `https://www.virustotal.com/gui/home/upload`
  - Buttons: OPEN
- **Download KeePassXC (password manager)** (`download-keepassxc` · link)
  - Description: Open-source offline password manager. Keeps all credentials in an encrypted local database — no cloud sync required.
  - What it does: Opens the site in your browser: `https://github.com/keepassxreboot/keepassxc/releases/tag/2.7.10`
  - Buttons: INSTALL, INSTALLER, PORTABLE, SITE
- **Download KeePass2Android (Android companion app)** (`download-keepass2android` · link)
  - Description: Android app that opens KeePass .kdbx databases. Sync the database file via cloud or LAN to use passwords on mobile.
  - What it does: Opens the site in your browser: `https://github.com/PhilippC/keepass2android/releases`
  - Buttons: OPEN
- **Download VeraCrypt (disk encryption)** (`download-veracrypt` · link)
  - Description: Open-source full-disk and container encryption. Successor to TrueCrypt, supports AES, Twofish, and Serpent.
  - What it does: Opens the site in your browser: `https://github.com/veracrypt/VeraCrypt/releases`
  - Buttons: INSTALLER, PORTABLE, SITE

#### 11.6 Media & Communication

- **Download Telegram Desktop Portable** (`download-telegram` · link)
  - Description: Portable version of Telegram Desktop — runs without installation, easy to move between PCs.
  - What it does: Opens the site in your browser: `https://desktop.telegram.org/`
  - Buttons: INSTALLER, PORTABLE, SITE
- **Download OBS Studio (screen recording & streaming)** (`download-obs` · link)
  - Description: Open-source screen recorder and live streaming software. Supports replay buffers, scene switching, and many plugins.
  - What it does: Opens the site in your browser: `https://github.com/obsproject/obs-studio/releases`
  - Buttons: INSTALLER, PORTABLE, SITE
- **Download 3D YouTube Downloader** (`download-3d-youtube-downloader` · link · side road)
  - Description: Convenient GUI downloader for YouTube, Vimeo, and many other video sites. Supports playlists and various quality options.
  - What it does: Opens the site in your browser: `https://yd.3dyd.com/download/`
  - Buttons: OPEN
- **Download MPC-HC (media player)** (`download-mpc-hc` · link · from Max.mov's guide)
  - Description: Lightweight open-source media player with MadVR, LAV Filters, and subtitle support. Successor to the original MPC-HC.
  - What it does: Opens the site in your browser: `https://github.com/clsid2/mpc-hc/releases`
  - Buttons: INSTALL, INSTALLER, PORTABLE, SITE
- **Download LosslessCut (fast video trimmer)** (`download-lossless-cut` · link)
  - Description: FFMPEG-based video trimmer that cuts video without re-encoding. Instant lossless cuts for any format.
  - What it does: Opens the site in your browser: `https://github.com/mifi/lossless-cut/releases`
  - Buttons: PORTABLE, SITE
- **Download DaVinci Resolve (free professional video editor)** (`download-davinci-resolve` · link)
  - Description: Industry-standard professional video editing suite by Blackmagic Design. Free version has no watermarks or time limits.
  - What it does: Opens the site in your browser: `https://www.blackmagicdesign.com/products/davinciresolve`
  - Buttons: OPEN
- **Download Obsidian (note-taking app)** (`download-obsidian` · link)
  - Description: Obsidian is a powerful Markdown-based note app with a local-first graph of linked notes. No account required for local use.
  - What it does: Opens the site in your browser: `https://github.com/obsidianmd/obsidian-releases/releases`
  - Buttons: INSTALLER, SITE

#### 11.7 Overclocking & Benchmarking

- **Download MSI Afterburner (GPU overclocking & monitoring)** (`download-msi-afterburner` · link)
  - Description: GPU overclocking, fan control, and in-game overlay tool. Works with all GPU brands despite the MSI name.
  - What it does: Opens the site in your browser: `https://www.msi.com/Landing/afterburner/graphics-cards`
  - Buttons: INSTALLER, SITE
- **Download Superposition Benchmark (GPU stress test)** (`download-superposition` · link)
  - Description: Extreme GPU benchmark by Unigine. Tests GPU stability under heavy load with a visually impressive scene.
  - What it does: Opens the site in your browser: `https://benchmark.unigine.com/superposition`
  - Buttons: INSTALLER, SITE
- **Download TestMem5 (RAM stability tester)** (`download-testmem5` · link · side road)
  - Description: TestMem5 is a fast RAM stress-testing tool popular for validating memory overclocks. Faster than MemTest86 for quick validation.
  - What it does: Opens the site in your browser: `https://github.com/CoolCmd/TestMem5/releases`
  - Buttons: PORTABLE, SITE
- **Download CapFrameX (frame capture & analysis)** (`download-capframex-bench` · link · from Max.mov's guide)
  - Description: CapFrameX captures frametimes with an in-game overlay and provides detailed FPS/frametime analysis and sensor logging.
  - What it does: Opens the site in your browser: `https://github.com/CXWorld/CapFrameX/releases`
  - Buttons: INSTALLER, PORTABLE, SITE

#### 11.8 Audio

- **Download SoundSwitch (hotkey audio device switcher)** (`download-soundswitch` · link · from Max.mov's guide)
  - Description: SoundSwitch lets you switch between audio output and input devices with a configurable keyboard shortcut.
  - What it does: Opens the site in your browser: `https://github.com/Belphemur/SoundSwitch/releases`
  - Buttons: INSTALLER, PORTABLE, SITE

#### 11.9 Gaming & Account Switching

- **Download TcNo Account Switcher** (`download-tcno-account-switcher` · link · from Max.mov's guide · side road)
  - Description: Multi-account switcher for Steam, Epic Games, EA, Origin, Riot, Ubisoft, and more.
  - What it does: Opens the site in your browser: `https://github.com/TCNOco/TcNo-Acc-Switcher/releases`
  - Buttons: INSTALLER, PORTABLE, SITE
- **Download Fan Control (cooling management)** (`download-fan-control-recommended` · link · from Max.mov's guide)
  - Description: Fan Control provides detailed fan curve configuration for system cooling. Listed here as a general recommended program.
  - What it does: Opens the site in your browser: `https://github.com/Rem0o/FanControl.Releases/releases`
  - Buttons: INSTALLER, PORTABLE, SITE
- **Download qBittorrent (torrent client)** (`download-qbittorrent` · link)
  - Description: Free, open-source torrent client without bundled adware. Full-featured with search, RSS, and sequential download.
  - What it does: Opens the site in your browser: `https://www.qbittorrent.org/download`
  - Buttons: INSTALL, INSTALLER, SITE

#### 11.10 Smartphone + PC Ecosystem

- **Phone Link — Microsoft ecosystem for Android** (`smartphone-pc-phone-link` · manual step · from Max.mov's guide)
  - Description: Microsoft Phone Link integrates your Android phone with Windows: shared clipboard, notifications, calls, and file transfer. See Section 5 for full setup instructions.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: For full setup instructions refer to Section 5 (Windows Settings) → Phone Link subsection.
- **Download KDE Connect (cross-platform phone/PC integration)** (`download-kde-connect` · link)
  - Description: KDE Connect provides clipboard sync, file transfer, notifications, and remote input between Windows and Android/Linux. Open-source alternative to Phone Link.
  - What it does: Opens the site in your browser: `https://kdeconnect.kde.org/`
  - Buttons: INSTALLER, SITE
- **Download Plain App (self-hosted phone/PC bridge)** (`download-plain-app` · link)
  - Description: Plain App is an open-source Android app that exposes your phone as a local web server for file management, clipboard sync, and SMS from your PC browser. No cloud account needed.
  - What it does: Opens the site in your browser: `https://github.com/ismartcoding/plain-app/tags`
  - Buttons: OPEN

### 12. Mouse & Keyboard Settings

#### 12.0 Mouse Settings

- **Gaming mouse myths — 8000 Hz polling, high DPI, mouse acceleration (YouTube)** (`mouse-myths-guide` · link · from Max.mov's guide · side road)
  - Description: Video guide debunking common gaming mouse myths: ultra-high polling rates, high DPI advantages, and mouse acceleration effects. Helps understand what settings actually matter for gaming.
  - What it does: Opens the site in your browser: `https://www.youtube.com/watch?v=2leo5S5RzRw`
  - Buttons: OPEN
- **Mouse settings guide — DPI, Angle Snap, Ripple Control, polling rate, etc. (Telegram)** (`mouse-settings-guide` · link · from Max.mov's guide · side road)
  - Description: Detailed guide covering optimal mouse DPI, Angle Snap, Ripple Control, polling rate selection, and other mouse-specific settings for gaming.
  - What it does: Opens the site in your browser: `https://t.me/allp1ay/1211`
  - Buttons: OPEN

#### 12.1 Keyboard Settings

- **Magnetic keyboard setup guide — Rapid Trigger, actuation point, Snap Tap, etc. (YouTube)** (`magnetic-keyboard-guide` · link · from Max.mov's guide · side road)
  - Description: Video guide for configuring Hall-effect / magnetic keyboards: Rapid Trigger, actuation height, Snap Tap (simultaneous opposite directions), and other advanced features.
  - What it does: Opens the site in your browser: `https://www.youtube.com/@MAXiM0V/videos`
  - Buttons: OPEN

### 13. Max.mov Tweaks

#### 13.0 Max.mov Hub

- **What belongs in Max.mov Tweaks** (`maxmov-what-is-this` · manual step · side road)
  - Description: A separate place for the Max.mov pack: profiles, wallpapers, cursor packs, personalization files, and community gaming notes.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Use this section as a hub for Max.mov-specific assets. Official vendor settings stay in their vendor sections; Max.mov packs, profiles, wallpapers, cursors, and gaming notes live here.
- **Open current Audion Setup Tools folder** (`maxmov-open-current-folder` · script)
  - Description: Opens the current portable project folder.
  - What it does: A script from the manifest. Its state is checked.
  - Buttons: OPEN
  - Note: Direct action: opens the current toolkit folder.
- **Open local Max.mov resources** (`maxmov-open-resources-folder` · script · side road)
  - Description: Opens the local Assets\MaxMov folder copied into this project.
  - What it does: A script from the manifest. Its state is checked.
  - Buttons: OPEN RESOURCES
  - Note: Direct action: opens the local Max.mov resources folder.

#### 13.1 Profiles & Presets

- **Open local personalization pack** (`maxmov-open-personalization-pack` · script · side road)
  - Description: Opens the local Max.mov personalization area with cursor packs, icons, wallpapers, and visual presets.
  - What it does: A script from the manifest. Its state is checked.
  - Buttons: OPEN
  - Note: Direct action: opens local copied resources. Review files manually before applying anything.
- **Open local Nvidia profiles/settings** (`maxmov-open-nvidia-profile-pack` · script · side road)
  - Description: Opens local copied Nvidia settings materials. Use them as reference for profiles and guides, while official NVIDIA recommendations remain in GPU & Monitor.
  - What it does: A script from the manifest. Its state is checked.
  - Buttons: OPEN
  - Note: Direct action: opens local Nvidia notes only. Donor URL shortcuts are not shipped.
- **Profiles are review-first** (`maxmov-profiles-note` · manual step · side road)
  - Description: Old Max.mov profiles should be reviewed before import because driver versions, game builds, and monitor setups change.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Do not bulk-import old profiles blindly. Open the local resource folder, check the target app/driver version, then apply only the relevant profile through the official tool or the app that created it.
- **Legacy scripts are not shipped here** (`maxmov-legacy-files-removed-note` · manual step)
  - Description: Old .reg, .bat, .cmd, .ps1, executables, archives, URL shortcuts, and shortcut files from the donor pack are intentionally excluded from Assets\MaxMov so users cannot run or open them by accident.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: If an old Max.mov operation is useful, convert it into a manifest card with PS-native Apply/Detect/Revert logic first. Do not restore legacy scripts as clickable files inside the resources folder.

#### 13.2 Wallpapers, Cursors & Visuals

- **Open local wallpapers/cursors** (`maxmov-open-wallpapers-cursors` · script · side road)
  - Description: Opens the local Max.mov visual assets area with .cur/.ani cursors and image files.
  - What it does: A script from the manifest. Its state is checked.
  - Buttons: OPEN
  - Note: Direct action: opens the local visual asset area.
- **Install cursor packs manually** (`maxmov-cursor-install-note` · manual step)
  - Description: Cursor packs should be applied through Windows Mouse Properties or the pack installer if one is included.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Open Settings -> Bluetooth & devices -> Mouse -> Additional mouse settings -> Pointers. Save your current scheme first, then browse to .cur/.ani files from the Max.mov cursor pack and save the new scheme.
- **Apply wallpapers through Personalization** (`maxmov-wallpaper-install-note` · manual step)
  - Description: Wallpapers are safe visual assets; apply them from Windows Personalization or copy them into your Pictures/Wallpapers folder.
  - What it does: A manual step: the program changes nothing, you do it.
  - Instruction: Copy the wallpapers you want to keep into your user Pictures or Wallpapers folder, then open Settings -> Personalization -> Background and select the image or slideshow folder.

#### 13.3 Gaming Pack

- **Open local Gaming resources** (`maxmov-open-gaming-folder` · script · side road)
  - Description: Opens the local Max.mov gaming resources folder with launcher notes, Timer Resolution notes, and FPS/latency notes.
  - What it does: A script from the manifest. Its state is checked.
  - Buttons: OPEN
  - Note: Direct action: opens the local gaming resources folder.
- **Open local Steam/Game Launchers pack** (`maxmov-open-steam-launchers-pack` · script · side road)
  - Description: Opens local copied notes for Steam and other game launchers. Donor URL shortcuts are not shipped; official download buttons stay in Steam & Game Launchers.
  - What it does: A script from the manifest. Its state is checked.
  - Buttons: OPEN
  - Note: Direct action: opens local launcher materials. Installers and official links still stay in Steam & Game Launchers.
- **Open local FPS/latency testing pack** (`maxmov-open-latency-testing-pack` · script · side road)
  - Description: Opens the local FPS and latency testing resources with Max.mov notes. Donor URL shortcuts are not shipped; official download buttons stay in FPS & Latency Testing.
  - What it does: A script from the manifest. Its state is checked.
  - Buttons: OPEN
  - Note: Direct action: opens local measurement materials. Tool downloads still live in FPS & Latency Testing.
- **Open local Timer Resolution notes** (`maxmov-open-timer-resolution-pack` · script · side road)
  - Description: Opens local Timer Resolution notes only. The old executable archive is intentionally not shipped here.
  - What it does: A script from the manifest. Its state is checked.
  - Buttons: OPEN
  - Note: Direct action: opens local Timer Resolution notes without installing or launching anything.
<!-- card-map:end -->

## Operational Safety Reference

Treat every card according to its action type. An `open` action only navigates to a Windows page, folder, URL, or external utility. A `script` action may change system state. A manual checklist requires the operator to complete and verify the steps. Do not assume that opening a tool applies its recommended configuration.

Before disk, boot, registry, firmware, driver-removal, privacy, or security-policy changes, create a tested rollback path. Depending on the operation this may be a restore point, exported registry branch, driver package, BitLocker recovery key, system image, BIOS profile, or separate backup.

## Recommended Order

Use the sections as a dependency-aware sequence: establish Windows and recovery first, update chipset/network/storage/display drivers, verify disks, configure core Windows behavior, then tune browsers, GPU, monitor, cooling, games, input devices, and optional latency features. Benchmark before and after optional tweaks.

Do not apply several performance changes at once. A one-change-at-a-time approach makes instability, latency regression, or power/thermal problems reversible and measurable.

## Driver And Update Review

Confirm hardware IDs and system model before installing a driver. Prefer the device or OEM source when it provides platform-specific packages. Create a recovery path before firmware or storage-controller updates. After installation, review Device Manager, Event Viewer, reboot requirements, and the actual driver version.

Avoid generic driver-removal or cleanup operations when the current package is needed for recovery, display output, network access, or storage boot.

## Disk And Storage Review

Verify disk identity by model, serial, capacity, and existing partitions, not drive letter alone. Drive letters and removable-disk order may change. Back up data and recovery keys before initialization, conversion, partitioning, formatting, encryption, or destructive cleanup.

After storage changes, verify boot, BitLocker state, filesystem health, expected partitions, free space, and backup accessibility.

## Display, GPU, And Cooling

Record the baseline resolution, refresh rate, color mode, GPU driver, temperatures, fan behavior, and stability. Apply display and performance changes separately. Confirm that the monitor is using the intended connection and refresh rate and that cooling remains safe under sustained load.

Overclocking, undervolting, fan curves, and power limits require measurement and a known reset path. A profile stable in a short benchmark may fail during long gaming or compute workloads.

## FPS And Latency

Use consistent test scenes, background processes, power plans, and measurement tools. Compare averages, lows, frame-time consistency, input latency, temperature, clocks, and power rather than one peak FPS value.

Timer-resolution and latency tools are optional. Do not stack several tools or leave an undocumented background utility running. Confirm that changes are removed after reboot or Exit when that is the intended behavior.

## Network, Browser, And Privacy

Review account, synchronization, proxy, DNS, certificate, extension, telemetry, and policy implications before applying a preset. Enterprise-managed devices may restore settings or reject local changes. Do not weaken browser or Windows security merely to remove a warning.

## Recovery After A Problem

Stop applying new cards. Record the last change, symptoms, reboot state, and relevant logs. Use the narrowest rollback: restore the previous setting or driver before using a broad system restore. A setting the program changed comes back with REVERT on its card; the value Windows has out of the box comes back with WINDOWS DEFAULT. For boot or storage failures, use prepared recovery media and keys rather than improvised destructive commands.

## Completion Checklist

- Windows Update and Device Manager show no unexplained failures.
- Storage, encryption, backup, and recovery paths are verified.
- Display mode, refresh rate, color, and scaling are correct.
- Cooling is stable under load.
- Optional performance changes have before/after measurements.
- Startup, tray, timer, and background utilities are known.
- Important configuration and rollback notes are recorded.

## Interface Map Maintenance

The manifests are the structured source for section order, card titles, controls, defaults, warnings, and action commands. The map above is built by `Docs\tools\Build-GuideMap.ps1` - run it after any change of a manifest; the map is not edited by hand. The rest of this guide remains the human-readable layer: it explains prerequisites, risk, expected effect, verification, and rollback.

A release review should verify that every visible action has a meaningful description and that dangerous operations identify elevation, reboot, service, driver, registry, disk, privacy, or network impact before execution. `MaxMovGuide = $true` records exact presence in the guide; add `MaxMovApproved = $true` only after separate explicit approval by Max.mov. Both fields affect card labels only, never execution. `Weight = 'essential' | 'danger'` gives a card the ESSENTIAL or CAREFUL mark; `WindowsDefault` is the out-of-the-box Windows value for WINDOWS DEFAULT (`'absent'` - remove the value, a startup type for services, `'balanced'` for power schemes, `'revert'` for scripts).
