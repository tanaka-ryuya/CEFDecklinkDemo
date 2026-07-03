# CEFDecklink - 广播级网页图形输出软件 (Demo Release)

语言: [English](README.md) | [日本語](README.ja.md) | [简体中文](README.zh.md)

CEFDecklink 是一款**轻量、单一功能且高度稳定的广播级软件。它使用嵌入式 Chromium (CEF) 渲染网页，并通过 SDI 接口以 1080i59.94 或 1080i50 的输出率，输出为“Fill（填充）和 Key（键）- 直切阿尔法 (Straight Alpha)”信号。**

只需将笔记本电脑连接到 Blackmagic **UltraStudio HD Mini** 或类似的 DeckLink 设备，即可在视频切换台上实现完美的半透明图形（字幕或 CG）合成。

---

## 📥 下载
请从本仓库下载并解压 **`CEFDecklink.zip`**。
解压后文件夹中的 `DeckLinkDX11.exe` 即为主程序（无需安装）。

*※ 使用 GitHub LFS (Large File Storage) 进行管理。*

> [!TIP]
> **无需硬件，即可立即体验！**  
> 即使手头没有 UltraStudio 或 DeckLink 设备，只要在 Windows 电脑上运行，软件就会自动以“模拟器模式”（在 PC 上的预览窗口中进行渲染）启动。无需连接任何硬件或复杂设置，即可立即在任意 Windows PC 上测试 HTML/WebGL 动画的流畅度以及修改配置。

---

## ✨ 主要功能

1. **Unmultiplied Keying (内置消除黑边功能)**  
   内置 "Unmultiplied" 滤镜，完美解决在切换台上合成半透明网页渐变或投影时边缘发黑（黑边/Fringe）的问题。实现完美的直切阿尔法 (Straight Alpha) 合成。
2. **60fps Free-Run CEF (平滑动画)**  
   内部浏览器 (CEF) 以 60fps 自主运行，并使用异步、无锁队列与 29.97fps (59.94i) 的硬件输出完美同步。防止 HTML 动画卡顿或隔行抖动。
3. **模拟器模式**  
   如果在没有连接 DeckLink 设备的 PC 上启动，程序会自动切换到“模拟器模式（窗口预览）”。即使没有硬件，也可以预览网页图形的渲染效果。

---

## 🚀 快速上手

### 1. 硬件连接
1. 通过 雷电 (Thunderbolt) 接口将 Windows PC 连接到 **UltraStudio HD Mini**。
2. 使用 SDI 线缆将 UltraStudio 的 **SDI OUT 1** 连接到切换台的 **Fill 输入**，将 **SDI OUT 2** 连接到 **Key 输入**。

### 2. 启动与设置
在 `DeckLinkDX11.exe` 同级目录下创建 `config.json` 配置文件，以指定目标网页的 URL 和输出格式。

**`config.json` 示例:**
```json
{
  "url": "https://cefdecklink-demo-page.vercel.app/sample.html",
  "unmult_thresh": 0.0,
  "format": "5994i",
  "il_filter_mode": 1
}
```

- `url`: 要渲染输出的网页 URL（支持本地路径 `file://...`）。
- `unmult_thresh`: Unmultiplied 处理的阈值（默认 `0.0` 表示纯直切阿尔法）。如果出现黑边，可在 `0.01` 到 `0.05` 之间微调。
- `format`: SDI 输出格式。仅支持 `"5994i"` (1080i59.94) 或 `"50i"` (1080i50)。
- `il_filter_mode`: 隔行扫描垂直低通滤波器（默认 `1` = 3-tap）。可根据线闪烁情况修改为 `0`（无）或 `2`（5-tap）。

运行 `DeckLinkDX11.exe` 即可开始输出。

---

## 🔒 安全及启动警告

- **SmartScreen 警告:**  
  本软件未进行数字签名。如果 Windows Defender SmartScreen 拦截，请点击**“更多信息”**，然后选择**“仍要运行”**。
- **防火墙警报:**  
  程序会为浏览器 DevTools 调试连接打开一个端口。即使点击**“取消（阻止）”**，输出功能仍可正常工作。

---

## ⌨️ 操作与功能 (控制台 TUI 快捷键)
当控制台窗口处于活动状态时，您可以使用以下按键操作：

- **`Ctrl + I`**: **隔行模式 (默认)** (合并前后帧以进行 1080i 输出)
- **`Ctrl + D`**: **Diff 模式** (可视化帧之间的差异，用于调试动画卡顿)
- **`Ctrl + P`**: **逐行模式** (用于预览窗口显示)
- **`Ctrl + A` / `Ctrl + Z`**: 微调 Unmult 阈值 (A 增加，Z 减少)
- **`Ctrl + C`**: 安全退出
- **`F11`**: 切换预览窗口全屏

---

## 🔑 专业版许可
默认情况下，输出画面的右下角会显示红色水印。
若要去除水印，请在可执行文件同级目录下放置 `licensekey.json`。

**`licensekey.json` 示例:**
```json
{
  "license_key": "YYYYMMDD-xxxxxxxxxxxxxxxx"
}
```
*※ 专业版许可证管理系统目前正在开发中。*

---

## 🛠️ 故障排除

- **显示 "Shader Compile Failed"**  
  请确保 `shaders` 文件夹已复制到可执行文件的同级目录下。
- **显示 "Decklink not found"**  
  未安装 DeckLink 驱动或设备未连接。程序将自动在模拟器模式下启动。
- **启动时崩溃**  
  请检查 ZIP 中的所有 CEF 依赖文件（如 `libcef.dll`）是否都完整地保留在当前目录下。

---

## ⚖️ 许可条款
本软件为专有软件。严禁二次分发、重新发布、反向工程和修改。使用前请务必阅读完整的 [使用条款 (LICENSE.zh.md)](LICENSE.zh.md)。
其他语言版本: [English LICENSE](LICENSE.md) | [日本語 LICENSE](LICENSE.ja.md)
