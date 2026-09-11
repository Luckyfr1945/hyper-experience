# HYPER EXPERIENCE

All-in-one module for Magisk, KernelSU, and APatch: EAS performance tuning, hardware bypass charging, adaptive thermal management, pure 8GB LZ4 ZRAM, and TCP BBR network turbo.

---

## Key Features

### Charging & Bypass
- **True Hardware Bypass (Net 0mA):** Powers motherboard directly from the USB input during gaming without charging the battery. Zero battery heat and battery percentage stays flat (no drain).
- **100% Full Battery Bypass:** Once battery hits 100%, charging current cuts to 0mA and powers the device directly via USB. Eliminates trickle charge heat and prevents battery degradation or swelling.
- **12-Tier Thermal Ladder:** Charging current steps down smoothly from 8.65A to 1.15A based on real-time battery temperature. Hard safety cut-off at 48°C (re-engages below 46°C).
- **Smart Charger Detection:** Auto-identifies USB PD, QC, DCP, and PC USB ports (SDP capped safely to 1.5A).

### Performance & Display
- **Dual-Mode Refresh Rate:** Locked 60Hz for daily browsing (maximum battery life), automatically switches to 120Hz/144Hz upon launching games.
- **Dynamic OPP Discovery:** Reads real hardware frequency tables directly from the kernel instead of relying on hardcoded MHz tables.
- **Touch Boost:** Bumps touch sampling rate to 360Hz/480Hz in games for minimal input latency.
- **SurfaceFlinger RT (FIFO 16):** Real-time UI rendering priority to eliminate micro-stutters and dropped frames.
- **Native C Daemon (hyper_ai):** Lightweight background binary monitoring real-time GPU load and thermal velocity (dT/dt) to prevent sudden thermal throttling.

### RAM & Storage
- **Pure 8GB LZ4 ZRAM:** 100% in physical RAM with zero flash wear (no disk swapfile on UFS storage).
- **Modern LMKD Tuning:** Configured with PSI complete stall at 70ms and thrashing limit at 30.
- **Smart Memory Trimmer:** Periodically trims cached buffers from bloated background apps (TikTok, Shopee, Instagram) without killing them or losing your scroll state.
- **RAM Pinning:** Keeps critical messaging apps (WhatsApp, Telegram) locked in memory to prevent missed notifications.

### Network
- **TCP BBR Engine:** Auto-probes and applies BBRv3/v2/BBR/Westwood for low-latency gaming.
- **Anti-Bufferbloat:** Enforces fq_codel / fq_pie queue discipline to eliminate bufferbloat spikes.
- **Wi-Fi Power Save Killer:** Disables Wi-Fi power-saving sleep states during gameplay for zero-jitter ping.
- **DNS Switcher:** Switch private encrypted DNS via terminal (Cloudflare, AdGuard, Google, Quad9).

### WebUI (KernelSU / APatch)
- Industrial telemetry HUD: Real-time display of battery temperature, current (mA), voltage, ZRAM usage, and bypass status.
- 12-Stage dynamic LED thermal matrix.
- One-click power mode toggles (Powersave / Balanced / Performance) and instant Hot-Reload without rebooting.

---

## Charging Thermal Ladder

| Battery Temp | Tier | Max Current | Description |
|---|---|---|---|
| < 36.9°C | `ULTRA` | 8.65A | Peak charging speed while cool |
| 36.9°C – 37.9°C | `COOL` | 8.65A | Sustained fast charge |
| 37.9°C – 38.9°C | `COOL LOW` | 7.65A | Gradual ramp down |
| 38.9°C – 39.9°C | `NORMAL` | 6.65A | Balanced daily charging |
| 39.9°C – 40.9°C | `NORMAL LOW` | 6.15A | Transition zone |
| 40.9°C – 41.9°C | `WARM LOW` | 4.15A | Controlled reduction |
| 41.9°C – 42.9°C | `WARM LIGHT`| 3.65A | Heat mitigation |
| 42.9°C – 43.9°C | `WARM` | 3.15A | Stable warm current |
| 43.9°C – 44.9°C | `WARM HIGH` | 2.65A | High warmth protection |
| 44.9°C – 45.9°C | `HOT` | 2.15A | Aggressive cooldown |
| 45.9°C – 46.9°C | `VERY HOT` | 1.65A | Emergency throttle |
| 46.9°C – 47.9°C | `CRITICAL` | 1.15A | Minimum current floor |
| ≥ 48.0°C | `CUTOFF` | 0A | Charging stopped! Resumes below 46°C |

---

## Terminal CLI (hypercharge)

Run in any root shell (Termux, MT Terminal, etc.):

```bash
su -c hypercharge [option]
```

- `hypercharge status` : Check active service status, thermals, battery flow, and recent logs.
- `hypercharge reload` : Hot-reload all module scripts in-place without rebooting.
- `hypercharge restart`: Restart background daemon cleanly.
- `hypercharge stop`   : Stop all module services and AI daemon.
- `hypercharge start`  : Start module background services.
- `hypercharge zram`   : Apply pure 8GB LZ4 ZRAM.
- `hypercharge trim`   : Run hardware UFS fstrim maintenance.
- `hypercharge pin`    : Pin WhatsApp & Telegram to RAM.
- `hypercharge net`    : Re-apply TCP BBR and gaming network tweaks.
- `hypercharge dns [cf|adguard|google|quad9|off]` : Change private DNS provider.

Example:
```bash
su -c hypercharge status
su -c hypercharge dns adguard
```

---

## Customization

- **AI Engine Config:** `/data/adb/modules/hyperexperience/hyper_ai.conf`  
  Adjust `gpu_boost_thresh` (default 80%) and thermal velocity trigger `rate_hot_thresh`.
- **Game Package List:** `/data/adb/modules/hyperexperience/game_list.sh`  
  Pre-loaded with 680+ games. Add any unlisted game package name to this list.

---

## Requirements & Compatibility

- **Root Manager:** Magisk v24+, KernelSU, APatch.
- **Android Versions:** Android 10 through Android 16.
- **SoC Architecture:** Qualcomm Snapdragon, MediaTek Dimensity/Helio, Samsung Exynos, Google Tensor.
- **ROM Compatibility:** AOSP, PixelOS, LineageOS, HyperOS/MIUI, ColorOS/OxygenOS, OneUI, etc.

---

## Installation

1. Flash `HYPER_EXPERIENCE_v5.2.28_UltraCool.zip` via Magisk / KernelSU / APatch.
2. Reboot device.
3. (Optional) Upgrades support **Live Hot-Reload** (active immediately upon flashing without reboot).

### Uninstallation:
Remove module via your root manager and reboot. All kernel governors, charging limits, and sysctl nodes restore to factory defaults.

---

## Credits & License
- **Original Author:** [Razal (Razal1_1)](https://t.me/Razal1_1)
- **Modifications & Enhancements:** [Kiki](https://github.com/)
- **License:** GNU General Public License v3 (GPLv3)
