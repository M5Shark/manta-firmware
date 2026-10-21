# M5Shark Firmware — Update v1.01

Official firmware update pack for M5Shark devices (ESP32 Marauder based).

## Devices covered

| Folder | Device | Chip |
|---|---|---|
| `firmware/C5-MANTA/` | M5Shark Manta (ESP32-C5, 4-antenna) | ESP32-C5 |
| `firmware/V6_1-SCREEN/` | V6.1 board with screen | ESP32 |
| `firmware/marauder_dev_board-FLIPPER/` | Marauder dev board (Flipper Zero) | ESP32 |
| `firmware/mini/` | Mini board | ESP32 |

Two app versions are included for V6.1-SCREEN: `v1_9_0` (stable) and `v1_11_0_beta` (beta) — flash the one you want.

---

## How to flash

### What you need
- Windows PC (for Flash Download Tool) or macOS/Linux terminal
- USB-C data cable
- The 3 (or 4) `.bin` files for your device from this repo

### Option A — Flash Download Tool (Windows, easiest)

1. Download **flash_download_tool** (v3.9.x) — a copy is in the [v1.01 release assets](../../releases), or get it from Espressif.
2. Connect the device over USB-C.
3. Open the tool and select your chip type:
   - **ESP32** for V6.1-SCREEN / Flipper board / mini
   - **ESP32-C5** for the Manta
4. Select the COM port (install the CH340 driver from the release assets if no port appears).
5. Add each file with its offset exactly as below for your device, tick the boxes, set SPI Speed to 40MHz / FlashSize 4MB (or detect), then press **ERASE** once, then **START**.
6. When it says FINISH, press the reset button on the device — done.

### Option B — esptool (any OS, command line)

Install: `pip install esptool` then run the command for your device (replace `COM7`/`/dev/tty.usbserial-*` with your port):

**Manta (ESP32-C5)** — erase first, then flash:
```
esptool.py --chip esp32c5 --port COM7 --baud 921600 write_flash -z \
  0x0 bootloader.bin \
  0x8000 partitions.bin \
  0x10000 esp32_marauder_v1_9_0_20251213_esp32c5devkitc1.bin
```

**V6.1-SCREEN / Flipper board / mini (ESP32)**:
```
esptool.py --chip esp32 --port COM7 --baud 921600 --before default_reset --after hard_reset write_flash -z \
  --flash_mode dio --flash_freq 80m --flash_size detect \
  0xe000 boot_app0.bin \
  0x1000 esp32_marauder.ino.bootloader.bin \
  0x10000 <app-file-for-your-device>.bin
```
App file per device:
- V6.1-SCREEN: `esp32_marauder_v1_9_0_20251213_v6_1.bin` (stable) or `esp32_marauder_v1_11_0_beta_20260227_v6_1.bin` (beta)
- Flipper board: `esp32_marauder_v1_9_0_20251213_marauder_dev_board_pro.bin`
- Mini: `esp32_marauder_v1_10_1_20260127_mini.bin`

### Flash offsets cheat-sheet (Flash Download Tool)

| File | ESP32 boards | Manta (C5) |
|---|---|---|
| `boot_app0.bin` | `0xe000` | — (not used) |
| `bootloader.bin` | `0x1000` | `0x0` |
| `partitions.bin` | — (built in) | `0x8000` |
| app `.bin` | `0x10000` | `0x10000` |

### Troubleshooting
- **No COM port:** install the CH340 driver (in release assets), replug the cable.
- **Failing to connect:** hold the **BOOT** button on the device while pressing START.
- **Won't boot after flash:** run ERASE once, then flash again.

---

## Downloads

Get the **full pack** (all firmware + Flash Download Tool + CH340 driver + video guide) from the [v1.01 release](../../releases/tag/v1.01).

## Support

Questions or issues: [m5shark.com](https://m5shark.com) · open an issue in this repo.
