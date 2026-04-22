# Scrcpy AppImage (非官方)

<div align="center">

![License](https://img.shields.io/github/license/Donjone/Scrcpy-AppImage)
![Version](https://img.shields.io/badge/scrcpy-v3.3.3-green)
![Platform](https://img.shields.io/badge/platform-Linux-blue)

[ [English] ](README.md) | **[ 中文说明 ]**

</div>

---

这是 [Genymobile/scrcpy](https://github.com/Genymobile/scrcpy) 的非官方 AppImage 发行版。

## 项目动因

本仓库旨在为那些通过原生包管理器（如 Fedora/RHEL 上的 `dnf`）安装 `scrcpy` 时遇到显示异常（如灰度或黑白画面）的系统提供替代安装方案。

通过将官方预编译的二进制文件打包为便携的 AppImage 格式，本版本将应用程序与宿主系统的依赖冲突彻底隔离，确保在各大 Linux 发行版上均能呈现正确的色彩和稳定的性能。

---

## 架构与特性

与普通的轻量级打包封装不同，本发行版被构建为一个**完全独立的全能包 (Standalone, All-in-one)**。它的运行完全不依赖于宿主系统的多媒体和调试基础库。

* **内置 FFmpeg (7.x)**：捆绑了现代的 FFmpeg 动态库，以确保视频渲染的稳定性，并完美兼容 Arch Linux、CachyOS 等滚动发行版。
* **独立 ADB (Standalone ADB)**：内置了最新版官方 Android Platform Tools。宿主系统完全无需预装 `android-tools`。
* **硬件加速**：通过 SDL2 自动映射并调用宿主机的原生显卡驱动，保障极低的延迟。

---
## 安装与使用

**💡 零依赖提示：** 由于这是全能自包含版本，您**无需**在宿主系统中提前安装 `adb`、`ffmpeg` 或 `SDL2` 等环境。所有核心组件均已内置，开箱即用。

### 选项 1：使用管理器安装（推荐）

此 AppImage 完全兼容 [Gear Lever](https://flathub.org/apps/it.mijorus.gearlever) 等主流的 Linux 应用程序管理器，这能帮你轻松地将它集成到桌面菜单并管理更新。

1. 在 [Releases](../../releases) 页面下载 `.AppImage` 文件。
2. 将文件导入 Gear Lever 或你偏好的管理器中。
3. 直接从系统的应用程序启动器中打开 `scrcpy`。

### 选项 2：命令行执行

1. 下载最新的 `.AppImage` 发布包。
2. 赋予可执行权限：
   ```bash
   chmod +x Scrcpy-x86_64.AppImage
   ```
3. 运行程序：
   ```bash
   ./Scrcpy-x86_64.AppImage
   ```

**🔧 故障排除 (AppImage 运行环境)：**
如果您在 Ubuntu 22.04+ 或其他较新的发行版上遇到类似 `dlopen(): error loading libfuse.so.2` 的报错，这是因为您的系统缺少运行 Type 2 AppImage 所需的 FUSE 2 库。
* **Ubuntu/Debian:** 运行 
```
sudo apt install libfuse2
```
* **Arch Linux:** 运行
```
sudo pacman -S fuse2
```
* **Fedora:** 运行 
```
sudo dnf install fuse
```

*(注意：目标 Android 设备必须处于开启 USB 调试的状态。)*
```

---

## 手动构建流程

如需在本地复现此构建，请参考 `.github/workflows/ci.yml` 中定义的自动化工作流。如果你倾向于使用标准工具手动打包基础二进制文件，可参考以下步骤：

### 环境准备
* `wget`
* `appimagetool`

### 操作步骤

1. **初始化 AppDir**：
   ```bash
   mkdir Scrcpy.AppDir
   # 将所需的二进制文件 (adb, scrcpy, scrcpy-server) 复制到 Scrcpy.AppDir/ 中
   ```

2. **配置桌面入口** (`Scrcpy.AppDir/scrcpy.desktop`)：
   ```ini
   [Desktop Entry]
   Name=scrcpy
   Type=Application
   Categories=Development;Utility;
   Terminal=false
   Exec=scrcpy
   Icon=icon
   Comment=Display and control your Android device
   ```

3. **定义启动入口**：
   ```bash
   cd Scrcpy.AppDir
   ln -s scrcpy AppRun
   cd ..
   ```

4. **编译 AppImage**：
   ```bash
   wget [https://github.com/AppImage/appimagetool/releases/download/continuous/appimagetool-x86_64.AppImage](https://github.com/AppImage/appimagetool/releases/download/continuous/appimagetool-x86_64.AppImage)
   chmod +x appimagetool-x86_64.AppImage
   ./appimagetool-x86_64.AppImage Scrcpy.AppDir
   ```

---

## 许可证

* **Scrcpy** 由 [Genymobile](https://github.com/Genymobile) 开发，采用 Apache 2.0 许可证。
* 本仓库仅提供用于在 Linux 环境中简化部署的打包脚本和构建配置。