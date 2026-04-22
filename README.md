
# Scrcpy AppImage (Unofficial)

<div align="center">

![License](https://img.shields.io/github/license/Donjone/Scrcpy-AppImage)
![Version](https://img.shields.io/badge/scrcpy-v3.3.3-green)
![Platform](https://img.shields.io/badge/platform-Linux-blue)

**[ English ]** | [ [中文说明] ](README_zh.md)

</div>

---

An unofficial AppImage distribution of [Genymobile/scrcpy](https://github.com/Genymobile/scrcpy).

## Project Rationale

This repository provides an alternative installation method for systems where native package managers (e.g., `dnf` on Fedora/RHEL) deliver problematic builds, such as those resulting in a grayscale or black-and-white rendering issue.

By packaging the official pre-built binaries into a portable AppImage format, this build isolates the application from host system dependency conflicts, ensuring correct color rendering and stable performance across various Linux distributions.

---

## Architecture & Features

Unlike standard lightweight wrappers, this release is built as a **standalone, all-in-one package**. It is designed to run independently of the host system's media and debugging libraries.

* **Integrated FFmpeg (7.x)**: Bundles modern FFmpeg libraries to ensure stable video rendering and compatibility with rolling-release distributions (e.g., Arch Linux, CachyOS).
* **Standalone ADB**: Includes the latest official Android Platform Tools. Host installation of `android-tools` is not required.
* **Hardware Acceleration**: Automatically maps and utilizes the host's native graphics drivers via SDL2.

---
## Installation & Usage

**💡 Zero-Dependency Note:** Because this is an All-in-One build, you do **not** need to install `adb`, `ffmpeg`, or `SDL2` on your host system. All core components are bundled inside.

### Option 1: AppImage Managers (Recommended)

This AppImage is fully compatible with application managers such as [Gear Lever](https://flathub.org/apps/it.mijorus.gearlever), which facilitate desktop integration and update management.

1. Download the `.AppImage` file from the [Releases](../../releases) page.
2. Import the file into Gear Lever or your preferred manager.
3. Launch `scrcpy` directly from your system's application launcher.

### Option 2: Command Line Execution

1. Download the latest `.AppImage` release.
2. Grant execution permissions:
   ```bash
   chmod +x Scrcpy-x86_64.AppImage
   ```
3. Execute the binary:
   ```bash
   ./Scrcpy-x86_64.AppImage
   ```

**🔧 Troubleshooting (AppImage Runtime):**
If you encounter a `dlopen(): error loading libfuse.so.2` error on modern distributions (like Ubuntu 22.04+), your system is missing FUSE 2, which is required to mount Type 2 AppImages.
* **Ubuntu/Debian:** Run
 ```
 sudo apt install libfuse2
 ```
* **Arch Linux:** Run 
```
sudo pacman -S fuse2
```
* **Fedora:** Run 
```
sudo dnf install fuse
```
*(Note: Target Android devices must have USB debugging enabled.)*


## Manual Build Process

To reproduce this build locally, refer to the automated workflow defined in `.github/workflows/ci.yml`. If you prefer to package the basic binaries manually using standard tools:

### Prerequisites
* `wget`
* `appimagetool`

### Procedure

1. **Initialize AppDir**:
   ```bash
   mkdir Scrcpy.AppDir
   # Copy required binaries (adb, scrcpy, scrcpy-server) into Scrcpy.AppDir/
   ```

2. **Configure Desktop Entry** (`Scrcpy.AppDir/scrcpy.desktop`):
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

3. **Define Entry Point**:
   ```bash
   cd Scrcpy.AppDir
   ln -s scrcpy AppRun
   cd ..
   ```

4. **Compile AppImage**:
   ```bash
   wget [https://github.com/AppImage/appimagetool/releases/download/continuous/appimagetool-x86_64.AppImage](https://github.com/AppImage/appimagetool/releases/download/continuous/appimagetool-x86_64.AppImage)
   chmod +x appimagetool-x86_64.AppImage
   ./appimagetool-x86_64.AppImage Scrcpy.AppDir
   ```

---

## License

* **Scrcpy** is developed by [Genymobile](https://github.com/Genymobile) and is licensed under the Apache 2.0 License.
* This repository provides packaging scripts and build configurations to facilitate deployment on Linux environments.