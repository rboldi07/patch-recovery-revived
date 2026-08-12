# Patch-Recovery-Revived — SM-A145R Community Fork

The only working `patch-recovery` tool that ever lived to patch Samsung's recovery images to enable **fastbootd mode**.

> **This is a community fork** of [ravindu644/patch-recovery-revived](https://github.com/ravindu644/patch-recovery-revived) with a workflow fix and device-specific notes for the **Galaxy A14 4G (SM‑A145R, MediaTek Helio G80)**.
>
> **Status: patching tested ✅ / flashing NOT yet tested after the fix ⚠️** — see the SM‑A145R section below.

<details>
<summary>Click to view image</summary>
<img src="./resources/1.jpg" alt="Preview" width="600"/>
</details>

---

## 🔧 Fix included in this fork

- **Fixed the `Failed to clean recovery directory` crash.** The original cleanup step did not remove hidden files from the work directory, which made the GitHub Action abort at startup with `[ERROR]: Failed to clean recovery directory`. This fork cleans the directory properly so the workflow can run.

---

## 📱 SM-A145R (Galaxy A14 4G / Helio G80) notes

### What is verified ✅
- The workflow successfully unpacks the stock `recovery.img` (full image with ramdisk, ~80 MB), applies **2 hex patches** to the `recovery` binary (`@ 0x00113A44` and `@ 0x00113B84`), and produces an ODIN‑flashable `SM-A145R-Fastbootd-patched-recovery.tar`.
- Firmware used: `[A145RXXSEDZE4]`

### What is NOT verified ⚠️
- The output `.tar` has **not been flashed after the fix in the script**. Whether "Enter fastboot" actually appears in recovery is unconfirmed.
- Flash at your own risk.

### Feedback wanted 🙏
If you flash this on an SM‑A145R, please **open an issue** with:
1. Your firmware build number
2. Whether fastbootd / "Enter fastboot" appears in recovery
3. Any bootloop or stock-restore behaviour

---

## 💡 Tips that made it work (read before running!)

1. **ZIP your image before uploading.** File hosts (catbox, tmpfiles, etc.) rename raw uploads to random names, and the script only accepts files literally named `recovery.img` or `vendor_boot.img`. Zipping preserves the name — the script unzips it automatically.
2. **Use a FULL recovery image.** If your `recovery.img` is small and has an empty ramdisk (common on newer One UI / Android 15 firmwares), there is nothing to patch. Decompress `recovery.img.lz4` from your AP tar and use the full image. If your device keeps the recovery ramdisk in `vendor_boot.img`, use that instead — the script supports both.

---

## Features

- Supports `.img`, `.lz4`, `.zip` and `.tar` formats as input.
- Supports both `recovery` and `vendor_boot` images.
- Automatically downloads and processes recovery images from a provided URL or local path.
- Hex-patches the recovery binary to enable **fastbootd** mode.
- **Boot image patching**: upload your `boot.img` together with `recovery.img`/`vendor_boot.img` to break the boot signature, preventing stock recovery auto-restoration on some devices.
- Creates an ODIN-flashable `.tar` file for easy flashing.

---

## Usage

### 🟢 GitHub Workflow

1. Start and fork this repository.
2. Trigger the workflow manually via the **Actions** tab.
3. Provide the inputs:
   - **Model**: your device's model number.
   - **Recovery Link**: direct download link to the image (or a `.zip` containing `recovery.img`/`vendor_boot.img` and optionally `boot.img`).
   - Hosts that work well: https://catbox.moe / https://filebin.net / GitHub Releases.

The workflow generates the patched image as an artifact and optionally uploads it to [GoFile](https://gofile.io/).

---

## ⚠️ Safety

- Unlocking the bootloader trips Knox and may void your warranty.
- Always keep the full stock firmware for your exact model/region saved before flashing anything modified.
- A bad recovery flash is usually recoverable via Download Mode + Odin with the stock `AP` tar.

---

## Credits

Developed by [@ravindu644](https://github.com/ravindu644).
Got the idea from [phhusson](https://github.com/phhusson), [Johx22](https://github.com/Johx22), [ratcoded](https://github.com/ratcoded).
Cleanup fix by [@rboldi07](https://github.com/rboldi07).
