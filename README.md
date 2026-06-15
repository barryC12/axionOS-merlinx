# AxionOS 2.4 SOLACE — Redmi Note 9 (merlinx) Flash Guide

> **Disclaimer:** I am not responsible for any damage to your device. Proceed at your own risk.

---

## Device Info

- **Device:** Redmi Note 9 (codename: `merlinx`)
- **SoC:** MediaTek Helio G85
- **ROM:** AxionOS 2.4 SOLACE (Android 16 QPR1) — Unofficial

---

## Requirements

- Windows PC
- High-quality USB-C cable
- [Android Platform Tools (adb/fastboot)](https://developer.android.com/tools/releases/platform-tools)
- [Xiaomi ADB USB Driver](https://xiaomitools.com/download/xiaomi-latest-usb-driver/?wpdmdl=4795&refresh=6a2d0b88ef5b41781336968&ind=1695152897715&filename=latest_usb_driver_windows.zip&lazykey=6a2d0b9592f88)
- [Latest ADB Fastboot Installer (fastboot driver)](https://github.com/fawazahmed0/Latest-adb-fastboot-installer-for-windows/releases/download/v1.7/Latest-ADB-Installer.bat)

---

## Step 1 — Enable Developer Options & OEM Unlock

> Skip this step if your bootloader is already unlocked.

1. Go to **Settings → About Phone**
2. Tap **MIUI Version** repeatedly until you get a popup saying you're a developer (enter your password if prompted)
3. Go back to **Settings** and search for **Developer Options**
4. Enable **USB Debugging** and **OEM Unlock**

---

## Step 2 — Install Drivers

### ADB Driver

1. Open **Device Manager**
2. Click the **Refresh** button — your phone should appear with an error next to it
3. Go to the **Action** tab → **Add Legacy Hardware**
4. Select **Manually** → highlight **All Devices** → press **Next**
5. Click **Have Disk** → **Browse** and select the Xiaomi ADB driver CAB fIle you downloaded
6. Install it

### Fastboot Driver

Run the `Latest-ADB-Installer.bat` file **as Administrator**.

---

## Step 3 — Verify ADB Connection

1. Connect your phone to the PC
2. Open a terminal in the directory containing `adb.exe`
3. Run:
   ```bat
   .\adb.exe -d devices
   ```
4. Accept the USB debugging prompt on your phone
5. Confirm your device shows up in the output

---

## Step 4 — Unlock the Bootloader

1. Reboot to bootloader:
   ```bat
   .\adb.exe -d reboot bootloader
   ```
2. Verify fastboot connection:
   ```bat
   .\fastboot.exe devices
   ```
3. Download the bootloader unlock tool from [this XDA thread](https://xdaforums.com/t/instant-bootloader-unlock-without-losing-data.4599949/)
4. Install the `.exe` files and **restart your PC**
5. Run `unlock-bootloader.bat`

If it succeeds, your phone's bootloader is now unlocked.

---

## Step 5 — Downgrade to MIUI 12.5.4 (if needed)

> Check your MIUI version under **Settings → About Phone**. If you're on **12.5.4 or lower**, skip this step.

### Flash PixelExperience Recovery

Reboot to bootloader, then:

```bat
.\fastboot.exe flash recovery PixelExperience_merlinx-13.0-20221113-1311-OFFICIAL.img
```

Download the recovery image: [PixelExperience merlinx recovery](https://get.pixelexperience.org/startDownload?url=https%3A%2F%2Fdl.pixelexperience.org%2FPv_GKKuO_pDekL3RihvybnUM6TxgOdCGo_C_gxJWR2kz_9o1Ow272uNHN223gc6n9_prw51CxMUUaLaPhy4FyQw-8Kem33IEGjIRBKsxjJM%3D%2FPixelExperience_merlinx-13.0-20221113-1311-OFFICIAL.img)

### Boot into Recovery

Hold **Power + Vol Up** until you boot into recovery.

### Format Data

In recovery, go to **Format** and format your data.

### Sideload MIUI 12.5.4

Download the MIUI zip: [miui_MERLINGlobal_V12.5.4.0.RJOMIXM](https://xm05.space/MyUrLiwlJzsOOD0BMz8GIywRNEEOFTAINBUpIzE/KzUOPDJBGyEELhsQMDUxIS0eGz8dLzE/Hx4OFQRBDjwfDDQ8Kw8bFQQeNBIuAA4KBCYzPC0wMQofHg4KHwwxED0VJwMHLx8QIi4OAh0vQikIJANCLTkDED0jMzwyCDwuMARCKQBAAyk0JjERQDUxBD0VJwMHLx8QIi4OAh0vQikIJANCLTkDMj01HyEGQRsCLQEfAikwPB4JAA4CHS8LCi1BHCVBAB8eFAAfAyk4Hx5CHg==)

In recovery go to **Apply Update → ADB Sideload**, then run:

```bat
.\adb.exe -d devices
.\adb.exe -d sideload miui_MERLINGlobal_V12.5.4.0.RJOMIXM_a4d0f9b695_11.0.zip
```

When prompted to reboot, confirm. Go through MIUI setup again.

---

## Step 6 — Flash the Custom Recovery

1. Enable **USB Debugging** again and connect to PC
2. Reboot to bootloader:
   ```bat
   .\adb.exe -d reboot bootloader
   ```
3. Download the recovery from the [axionOS-merlinx releases](https://github.com/barryC12/axionOS-merlinx/releases/tag/axionOS-merlinx)
   - **Without root:** `recovery.img`
   - **With root (Magisk):** [`recovery-magisk.img`](https://github.com/barryC12/axionOS-merlinx/releases/download/axionOS-merlinx/recovry-magisk.img)
4. Flash it:
   ```bat
   .\fastboot.exe flash recovery recovery.img
   ```
   or
   ```bat
   .\fastboot.exe flash recovery recovry-magisk.img
   ```
5. Manually reboot to recovery by holding **Power + Vol Up**

> ⚠️ Make sure you do this correctly on the first try — if you boot to MIUI instead, you'll need to re-flash the recovery via fastboot.

---

## Step 7 — Format Data & Flash AxionOS

In recovery:

1. Go to **Format** and run through all three format options (ignore any errors here)
2. Go to **Update → ADB Sideload**

Download the ROM: [`axion-2.4-SOLACE-20260319-UNOFFICIAL-VANILLA-merlinx.zip`](https://github.com/barryC12/axionOS-merlinx/releases/download/axionOS-merlinx/axion-2.4-SOLACE-20260319-UNOFFICIAL-VANILLA-merlinx.zip)

Then sideload it:

```bat
.\adb.exe -d sideload axion-2.4-SOLACE-20260319-UNOFFICIAL-VANILLA-merlinx.zip
```

When it finishes and asks to reboot:
- **If you want GApps or root → choose No** and continue below
- **If you want a vanilla install → choose Yes** and skip to [What Works](#what-works)

---

## Step 8 — (Optional) Flash GApps

Download: [`MindTheGapps-16.0.0-arm64-20260409_073023.zip`](https://github.com/MindTheGapps/16.0.0-arm64/releases/download/MindTheGapps-16.0.0-arm64-20260409_073023/MindTheGapps-16.0.0-arm64-20260409_073023.zip)

Go back to **Update → ADB Sideload**, then run:

```bat
.\adb.exe -d sideload MindTheGapps-16.0.0-arm64-20260409_073023.zip
```

---

## Step 9 — (Optional) Flash Magisk (Root)

Download: [`Magisk-v30.7.apk`](https://github.com/topjohnwu/Magisk/releases/download/v30.7/Magisk-v30.7.apk)

Go to **Update → ADB Sideload**, then run:

```bat
.\adb.exe -d sideload Magisk-v30.7.apk
```

Then reboot. If everything went correctly, you should boot into AxionOS.

---

## What Works

- Bluetooth
- Wi-Fi
- GPU acceleration
- Volume controls
- VoLTE
- All standard functionality

## What Doesn't Work

> Tell me if you run into anything.

---

## Unbrick Guide (Xiaomi Logo Boot Loop)

If your phone is stuck in a boot loop (Xiaomi logo → power off → repeat), use `mtkclient` to restore the recovery.

### Requirements

- A PC running **Arch Linux** or **WSL with Arch**

### Steps

```bash
sudo pacman -Syyu git
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
yay -S mtkclient
```

Download the recovery image: [PixelExperience merlinx recovery](https://dl.pixelexperience.org/Pv_GKKuO_pDekL3RihvybnUM6TxgOdCGo_C_gxJWR2kz_9o1Ow272uNHN223gc6n9_prw51CxMUUaLaPhy4FyQw-8Kem33IEGjIRBKsxjJM=/PixelExperience_merlinx-13.0-20221113-1311-OFFICIAL.img)

Then flash it with mtkclient:

```bash
sudo mtk w recovery ./PixelExperience_merlinx-13.0-20221113-1311-OFFICIAL.img
```

Follow the on-screen instructions. Once done:

- Boot to **recovery** → hold **Power + Vol Up**
- Boot to **fastboot** → hold **Power + Vol Down**

---

## Links

| Resource | Link |
|---|---|
| AxionOS merlinx releases | [github.com/barryC12/axionOS-merlinx](https://github.com/barryC12/axionOS-merlinx/releases/tag/axionOS-merlinx) |
| Platform Tools | [developer.android.com](https://developer.android.com/tools/releases/platform-tools) |
| Xiaomi ADB Driver | [xiaomitools.com](https://xiaomitools.com/download/xiaomi-latest-usb-driver/?wpdmdl=4795&refresh=6a2d0b88ef5b41781336968&ind=1695152897715&filename=latest_usb_driver_windows.zip&lazykey=6a2d0b9592f88) |
| Fastboot Installer | [github.com/fawazahmed0](https://github.com/fawazahmed0/Latest-adb-fastboot-installer-for-windows/releases/download/v1.7/Latest-ADB-Installer.bat) |
| Bootloader Unlock Tool | [XDA Thread](https://xdaforums.com/t/instant-bootloader-unlock-without-losing-data.4599949/) |
| MindTheGapps | [github.com/MindTheGapps](https://github.com/MindTheGapps/16.0.0-arm64/releases/download/MindTheGapps-16.0.0-arm64-20260409_073023/MindTheGapps-16.0.0-arm64-20260409_073023.zip) |
| Magisk | [github.com/topjohnwu/Magisk](https://github.com/topjohnwu/Magisk/releases/download/v30.7/Magisk-v30.7.apk) |
