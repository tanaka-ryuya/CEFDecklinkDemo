# CEFDecklink - Broadcast-Grade Graphic Playout Software (Demo Release)

Language: [English](README.md) | [日本語](README.ja.md) | [简体中文](README.zh.md)

CEFDecklink is a **lightweight, single-purpose, and highly stable broadcast-grade software that renders web pages using an embedded Chromium Embedded Framework (CEF) and outputs them as "Fill and Key (Straight Alpha)" signals via SDI at 1080i59.94 or 1080i50.**

Simply connect a laptop to a Blackmagic **UltraStudio HD Mini** or similar DeckLink device to achieve perfect semi-transparent graphic synthesis on video switchers.

---

## 📥 Download
Download and extract **`CEFDecklink.zip`** from this repository.
The executable `DeckLinkDX11.exe` in the extracted folder is the main application (no installation required).

*※ Managed using GitHub LFS (Large File Storage).*

> [!TIP]
> **Try it instantly without hardware!**  
> If you don't have an UltraStudio or DeckLink device, the Software will automatically launch in **"Simulator Mode"** (rendering in a preview window on your PC). You can test the smoothness of HTML/WebGL animations and check the configuration immediately on any Windows PC.

---

## ✨ Key Features

1. **Unmultiplied Keying (Built-in Fringe Prevention)**  
   Equipped with a built-in "Unmultiplied Filter" to eliminate edge darkening (fringe issues) when blending semi-transparent web gradients or drop shadows on switchers. Enables perfect straight alpha synthesis.
2. **60fps Free-Run CEF (Smooth Animations)**  
   Renders the internal browser (CEF) at 60fps autonomously and uses asynchronous, lock-free queues to achieve perfect synchronization with the 29.97fps (59.94i) hardware output. This prevents animation stutter and interlaced judder.
3. **Simulator Mode**  
   If launched on a PC without a DeckLink device, the app automatically falls back to "Simulator Mode (Window Preview)." You can verify web graphic rendering even without hardware.

---

## 🚀 Quick Start

### 1. Hardware Connection
1. Connect a Windows PC to **UltraStudio HD Mini** via Thunderbolt.
2. Connect UltraStudio **SDI OUT 1** to the switcher's **Fill input** and **SDI OUT 2** to the **Key input** using SDI cables.

### 2. Launch & Settings
Create a `config.json` file next to `DeckLinkDX11.exe` to specify the target web URL and output format.

**`config.json` Example:**
```json
{
  "url": "https://cef-decklink-sale.vercel.app/sample.html",
  "unmult_thresh": 0.0,
  "format": "5994i",
  "il_filter_mode": 1
}
```

- `url`: Web page URL to render (local paths like `file://...` are supported).
- `unmult_thresh`: Threshold for unmultiplied processing (default `0.0` for pure straight alpha). Adjust between `0.01` and `0.05` if fringing occurs.
- `format`: SDI output format. Only `"5994i"` (1080i59.94) or `"50i"` (1080i50) are supported.
- `il_filter_mode`: Interlace vertical low-pass filter (default `1` = 3-tap). Can be changed to `0` (None) or `2` (5-tap) depending on flicker.

Run `DeckLinkDX11.exe` to start playout.

---

## 🔒 Security & Startup Warnings

- **SmartScreen Warning:**  
  This software is not digitally signed. If Windows Defender SmartScreen blocks it, click **"More Info"** and then select **"Run anyway"**.
- **Firewall Alert:**  
  The app opens a port for browser DevTools connection. Even if you click **"Cancel (Block)"**, the playout function will work normally.

---

## ⌨️ Controls (Console TUI Shortcuts)
While the console window has focus, you can use the following keys:

- **`Ctrl + I`**: **Interlace Mode (Default)** (Merges frames for 1080i output)
- **`Ctrl + D`**: **Diff Mode** (Visualizes difference between frames for debugging stutter)
- **`Ctrl + P`**: **Progressive Mode** (For preview window rendering)
- **`Ctrl + A` / `Ctrl + Z`**: Fine-tune Unmult threshold (A to increase, Z to decrease)
- **`Ctrl + C`**: Safe exit
- **`F11`**: Toggle preview window fullscreen

---

## 🔑 Pro License
By default, a red watermark is displayed in the bottom-right corner.
To remove the watermark, place a `licensekey.json` next to the executable.

**`licensekey.json` Example:**
```json
{
  "license_key": "YYYYMMDD-xxxxxxxxxxxxxxxx"
}
```
*※ Pro license management system is currently under development.*

---

## 🛠️ Troubleshooting

- **"Shader Compile Failed"**  
  Ensure the `shaders` folder is copied next to the executable.
- **"Decklink not found"**  
  The DeckLink driver is not installed or the device is not connected. The app automatically starts in Simulator Mode.
- **App crashes on launch**  
  Verify that all CEF files (e.g., `libcef.dll`) from the ZIP are present in the directory.

---

## ⚖️ License
This software is proprietary. Redistribution, reverse engineering, and modification are strictly prohibited. Please read the full [LICENSE](LICENSE.md) before use.
For other languages: [日本語利用規約](LICENSE.ja.md) | [简体中文使用条款](LICENSE.zh.md).
