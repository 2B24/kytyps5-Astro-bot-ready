# ⚠️ Please don't ask me for any more fixes
## I did what I can. Maybe another developer can continue the rest.

---

# KytyPS5 – Astro Bot ready

A ready-to-play package of the **KytyPS5** PlayStation 5 emulator, set up and fixed for **ASTRO BOT** on Windows.
You add your own copy of the game; everything else is in the archive.

**[⬇ Download: Astro Bot PC by 2B.rar](https://github.com/2B24/kytyps5-Astro-bot-ready/raw/main/Astro%20Bot%20PC%20by%202B.rar)** (6.9 MB)

> [!IMPORTANT]
> **Disclaimer.** I do not own this emulator. It belongs to its developers ([credits below](#main-developer-and-owner)).
> I only set it up and added fixes so Astro Bot plays well. I am **not responsible** for anything that happens
> from using it: use it at your own risk.
> This project is not affiliated with Sony Interactive Entertainment or PlayStation. **No games, firmware or other
> copyrighted Sony software are included.** Use only a game you own and dumped from your own PS5.

---

## Main developer and owner

| | Project | Owner / developer |
|---|---|---|
| **Main emulator** | [KytyPS5](https://github.com/KytyPS5/KytyPS5) | Owned by the **KytyPS5** organization. Main developer: **[Nmzik](https://github.com/Nmzik)** |
| Original emulator | [Kyty](https://github.com/InoriRus/Kyty) | **[InoriRus](https://github.com/InoriRus)** |
| Build used here | [BryKytyPS5](https://github.com/BryanKAdams/BryKytyPS5), an Astro Bot performance fork of KytyPS5 | **[BryanKAdams](https://github.com/BryanKAdams)** |
| Astro Bot fixes and setup | this repository | 2B |

All the emulator work belongs to the people above. Please support the original projects.

---

## What was fixed

| Problem | What was wrong | Fix |
|---|---|---|
| **L2 + R2 did not accelerate the Dual Speeder** | The game asks the controller for the adaptive-trigger state before it acts on L2/R2. The emulator always answered "no effect", so the ship never moved. | The trigger state now follows the game's trigger effect and how far each trigger is pressed. Ported from KytyPS5 pull request [#1132](https://github.com/KytyPS5/KytyPS5/pull/1132) (Jetsku). |
| **Crash on some shader resource tables** | A null resource table the game passes made the emulator crash. | Null tables are read as zero, as on the console. |
| **Black silhouettes and broken lighting** | The emulator cannot do ray tracing, so the game's ray-traced lighting rendered black. | Includes the community non-ray-tracing game patch (KytyPS5 issue #745), applied automatically. |
| **Blurry picture on 4K screens** | With a 1920x1080 window the emulator tells the game it is on a 1080p TV, and the picture gets stretched. | The launcher starts fullscreen at your screen's resolution, so the game outputs real 4K. |
| **Freezes when new effects appear** | New shaders compiled while the game waited. | Settings tuned: shaders compile in the background, plus dynamic-resolution headroom (115%) so the game lowers its resolution before the frame rate drops. |
| **Texture correctness** | Two upstream bugs in texture tiling and copies. | Two upstream KytyPS5 fixes added (block-compressed textures in R64 tiling, tiled texture-array copies). |
| **Hard to start** | Long command lines, occasional failed starts. | Double-click launchers that check the game folder and retry the rare failed start. |
| **Very low FPS (5–15) in Sky Garden** | Another program held 2 GB of video memory. The game needs about 12–13 GB, so its data spilled into much slower system memory. | With that program closed, Sky Garden ran at 60 FPS again (13 ms per frame instead of 80–160 ms). Close GPU-heavy programs before playing. |

### What else was found while testing

- In busy areas the remaining slowdowns come from inside the emulator, not from the graphics card's speed. It ran at full clock speed in every test.
- **Not fixed:** the water in Sky Garden shows blocky, broken textures from some camera angles. This is an emulator bug.

---

## Requirements

- Windows 10 or 11, 64-bit
- A graphics card with Vulkan and up-to-date drivers; **16 GB of video memory recommended**
  (tested on an RTX 4070 Ti Super 16 GB with a Ryzen 7 7800X3D)
- [Microsoft Visual C++ Redistributable 2015–2022 (x64)](https://aka.ms/vs/17/release/vc_redist.x64.exe) (most PCs have it)
- Your own **ASTRO BOT** game files: **PPSA21567, version 01.019.000** (the patch only works with this version)
- A controller (DualSense recommended) or the keyboard

## How to use

1. Download **Astro Bot PC by 2B.rar** and extract it to its own folder, for example `C:\Games\Astro Bot`.
2. Copy the **contents** of your Astro Bot game folder into the **`Game`** folder, so that `Game\eboot.bin` exists.
3. Double-click **`Play Astro Bot.bat`** (fullscreen, 4K on a 4K screen), or **`Play Astro Bot - Window.bat`** (1920x1080 window, faster).
4. Quit with **Alt+F4** so the shader cache is saved. The first run stutters while shaders build; later runs are smoother.

| Key | Action |
|---|---|
| F2 | Emulator settings ("Dynamic resolution headroom": higher = steadier frame rate, lower = sharper) |
| F11 or Alt+Enter | Toggle fullscreen |
| Alt+F4 | Quit (saves the shader cache) |

Full instructions, keyboard controls and troubleshooting are in **README.txt** inside the archive.

**Performance:** in 4K, busy areas run at about 45 FPS on an RTX 4070 Ti Super; the window mode often holds 60.

---

## License and source code

The emulator is free software under the **GNU General Public License v2** ([LICENSE](LICENSE)).
This build is [BryKytyPS5](https://github.com/BryanKAdams/BryKytyPS5) at tag `BryKytyPS5-2026-10-04-a9e1ae5`
plus the changes in [`source/2B-changes.patch`](source/2B-changes.patch) (`git apply source/2B-changes.patch`).
