### Dreadnought T1 Series<br>DIY Installation Guide:
---
This guide documents the full bring-up process for the **Dreadnought T1**, it covers BIOS unlocking, Linux installation, and GPU governor setup. All three steps are required to take the board from a console-salvage state to a desktop-class Linux machine.
```text
- BIOS:      BC250_3.00_CHIPSETMENU.ROM
- Distro:    CachyOS — KDE Plasma edition (Arch)
- Governor:  PS5GPU-BC250 (Oberon-based GPU Graphics App)
```
---
### 1. BIOS Flashing — Method 1 (USB / EFI Shell):
---
> Flashing `BC250_3.00_CHIPSETMENU.ROM` is what unlocks **dynamic VRAM allocation** and the hidden **chipset menu** on the BC-250. This is the community-standard modded BIOS — stable, tested, and sufficient for everything the Dreadnought T1 needs. The full source guide is available at [elektricm.github.io/amd-bc250-docs/bios/flashing](https://elektricm.github.io/amd-bc250-docs/bios/flashing/).
```text
- USB Stick:  FAT32 formatted, 32GB or smaller recommended (Use Rufus to format bigger drives)
- SHA256:     48fbe5d366e6a56e2fdffdca848426216ba1f083610dab63db89d2f4e6c940b5
- Board:      Must be in working, POST-capable condition
- File:       BC250_3.00_CHIPSETMENU.ROM
```
 
**Step 1 — Download the files**
1. Flashing tools (EFI kit): `4U12G BIOS Update.zip`, from the [kenavru/BC-250](https://github.com/kenavru/BC-250) repository — contains `AfuEfix64.efi` and `Flash.nsh`.
2. Modded BIOS ROM: `BC250_3.00_CHIPSETMENU.ROM`, from [TuxThePenguin0's GitLab](https://gitlab.com/TuxThePenguin0/bc250-bios/). Verify the SHA256 above with `sha256sum` before flashing.
**Step 2 — Prepare the USB stick**
1. Format the stick to FAT32.
2. Extract the contents of `BIOS EFI` (from the zip) to the **root** of the stick.
3. Back up the stock `Robin5.00` file somewhere safe.
4. Copy `BC250_3.00_CHIPSETMENU.ROM` to the root of the stick and **rename it to `Robin5.00`** (no extension) — or edit `Flash.nsh` to match your filename instead.
**Step 3 — Boot to the EFI Shell**
1. Unplug all drives/SSDs from the board so it has no OS to boot into.
2. Insert the USB stick and power on — the board should drop directly into the yellow-on-black EFI Shell.
**Step 4 — Execute the flash**
```text
Shell> blk0:
Shell> Flash.nsh
```
> Add a space after the colon on `blk0:`. Once the AMI Firmware Update Utility starts, **do not touch the keyboard or power off the board** — even if it appears to hang, wait at least 15 minutes before assuming failure.
 
**Step 5 — Power down & remove USB**
Power off the board the moment the flash finishes, then remove the USB stick immediately to prevent an accidental re-flash on next boot.
 
**Step 6 — Clear CMOS (critical, do not skip)**
1. Remove the CR2032 CMOS battery for at least 60 seconds.
2. Optionally tap the power button a few times with the battery out to discharge residual capacitance.
3. Reinsert the battery.
**Step 7 — BIOS configuration**
Power on, spam **Del** to enter setup, confirm the clock has reset (proof CMOS actually cleared), then set:
 
| Setting | Location | Value |
|---|---|---|
| Integrated Graphics Controller | Chipset → GFX Configuration | Forces |
| UMA Mode | Chipset → GFX Configuration | UMA_SPECIFIED |
| UMA Frame Buffer Size | Chipset → GFX Configuration | 512MB (dynamic, recommended) |
| IOMMU | Advanced → CPU Configuration | Disabled |
 
Press **F10** to save and exit.
 
---
### 2. Operating System — CachyOS (KDE Plasma):
---
> CachyOS was chosen for the Dreadnought T1 for its BORE scheduler and x86-64-v3/v4-optimized packages, which give it the best measured gaming and general-desktop performance among tested BC-250 distros. As of late 2025 it installs directly from the standard ISO — no custom kernel builds required. Full source guide: [elektricm.github.io/amd-bc250-docs/linux/cachyos](https://elektricm.github.io/amd-bc250-docs/linux/cachyos/).
```text
- USB Stick:      8GB+
- Desktop:        KDE Plasma
- Bootloader:     GRUB
- Kernel target:  6.18.18 LTS (recommended) or 6.17.11+
- Avoid:          6.15.0–6.15.6 and 6.17.8–6.17.10 (broken GPU init)
```
 
**Step 1 — Create the installer**
1. Download the latest CachyOS ISO (**KDE Plasma** edition) from [cachyos.org](https://cachyos.org/).
2. Write it to the USB stick:
```bash
sudo dd if=cachyos.iso of=/dev/sdX status=progress conv=sync && sync
```
 
**Step 2 — Install**
1. Boot the Dreadnought T1 from the USB stick and launch the installer.
2. Partitioning: GPT with an EFI partition (auto or manual).
3. Desktop: **KDE Plasma**.
4. Bootloader: GRUB.
5. Kernel: leave the default selection — it should already be compatible.
**Step 3 — Verify the kernel**
```bash
uname -r
# Expect 6.18.x LTS (recommended) or 6.17.11+
```
> If the installer ISO won't boot (black screen or GPU panic), it likely shipped a broken kernel revision — fall back to installing `linux-cachyos-lts` from a working Arch base instead of the standard ISO.
 
**Step 4 — Post-install essentials**
```bash
# Sensors
sudo pacman -S lm_sensors
echo 'nct6683' | sudo tee /etc/modules-load.d/nct6683.conf
echo 'options nct6683 force=true' | sudo tee /etc/modprobe.d/sensors.conf
sudo mkinitcpio -P
 
# Gaming stack
sudo pacman -S steam mangohud goverlay gamemode gamescope
sudo pacman -S nvtop htop fastfetch
sudo pacman -S protonup-qt
```
 
---
### 3. GPU Governor — PS5GPU-BC250:
---
> The Cyan Skillfish GPU on the BC-250 is locked at a flat 1500MHz without a governor managing clock, voltage, and thermals. **PS5GPU-BC250** is an Oberon-based controller with a graphical interface (PT/EN/RU) offering manual clock/voltage control, a configurable thermal throttle/recovery curve, and a 4-stage automatic performance (OPP) table. It ships precompiled — no kernel patching required. Source: [github.com/ZEROAESQUERDA/PS5GPU-BC250](https://github.com/ZEROAESQUERDA/PS5GPU-BC250).
```text
- Base:        Oberon Governor
- Interface:   sudo ps5gpu-gui (PT / EN / RU)
- Install:     install.sh (traditional) or install2.sh (immutable — Bazzite/Bluefin)
- Note:        remove/disable any other GPU governor first to avoid conflicts
```
 
**Step 1 — Clone & install**
```bash
git clone https://github.com/ZEROAESQUERDA/PS5GPU-BC250.git
cd PS5GPU-BC250
 
# CachyOS is a traditional (non-immutable) system:
sudo bash install.sh
```
On success, the terminal prints a confirmation message. Reboot the board — the GUI launches automatically on startup afterward.
 
**Step 2 — Configure**
Open the interface manually at any time with:
```bash
sudo ps5gpu-gui
```
- **GPU status** is shown at the top of the interface.
- **Manual mode** hands clock and voltage control entirely to the user.
- **Thermal control** uses two temperatures: a *control* threshold (clock/voltage step down) and a *recovery* threshold (must be set lower than control, at which the clock climbs back up).
- **Automatic control** should normally stay **enabled**.
- The **OPP table** configures 4 automatic GPU performance stages.
**Step 3 — Verify**
```bash
pgrep -fl ps5gpu-gui
```
 
> Known limitation: the 350–2230MHz extended frequency table isn't exposed by default on most systems (adding it risks instability) — a future release may add it. Development has been tested primarily on CachyOS; Bazzite/Deck-mode systems may need extra tuning.
 
---
### Result:
---
> With BIOS unlocked, CachyOS/KDE installed on a supported kernel, and PS5GPU-BC250 managing the GPU, the Dreadnought T1 is ready for its intended workloads — desktop use, 1080p gaming, and local LLM inference up to ~10–12GB depending on VRAM split.
