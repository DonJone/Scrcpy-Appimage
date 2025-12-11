# Scrcpy AppImage (Unofficial)

<div align="center">

![License](https://img.shields.io/github/license/Donjone/Scrcpy-AppImage)
![Version](https://img.shields.io/badge/scrcpy-v3.3.3-green)
![Platform](https://img.shields.io/badge/platform-Linux-blue)

**[ English ]** | [ [中文说明] ](README_zh.md)

</div>

---

An unofficial AppImage build for [Genymobile/scrcpy](https://github.com/Genymobile/scrcpy).

## 🧐 Why this repository?

This repository was created to address a specific issue where installing `scrcpy` via system package managers (e.g., `dnf` on Fedora/RHEL) resulted in a **grayscale/black-and-white display**.

This AppImage packages the official pre-built binaries (v3.3.3) into a portable format. By bypassing the specific repository versions that cause the rendering glitch, this build ensures a **proper full-color display** on affected systems.

---

## ⚠️ Important Note on Dependencies

**This is a "Lightweight" AppImage.** Unlike standard AppImages that bundle every single library, this package wraps the pre-compiled binaries. It relies on your system having the basic runtime libraries installed.

To ensure it runs, you likely need the following installed on your system:
* `android-tools` (ADB)
* `SDL2`
* `ffmpeg` (libavcodec/libavformat)

If the AppImage fails to launch, please install the standard scrcpy dependencies via your package manager first (e.g., `sudo dnf install android-tools SDL2 ffmpeg`).

---

## 🚀 Installation & Usage

### Option 1: Manage with Gear Lever (Recommended)

This AppImage is fully compatible with [Gear Lever](https://flathub.org/apps/it.mijorus.gearlever), a popular AppImage manager for Linux. This allows you to easily integrate it into your app menu and manage updates.

1.  **Download** the `.AppImage` file from the [Releases](../../releases) page.
2.  Open **Gear Lever**.
3.  Click **"Open File"** and select the `Scrcpy-x86_64.AppImage`.
4.  Gear Lever will automatically move it to your AppImages folder and create a desktop entry.
5.  Launch `scrcpy` directly from your system's application launcher.

### Option 2: Manual Run

1.  **Download** the latest `.AppImage` file.
2.  **Make it executable**:
    ```bash
    chmod +x Scrcpy-x86_64.AppImage
    ```
3.  **Run it**:
    ```bash
    ./Scrcpy-x86_64.AppImage
    ```

*(Note: Ensure USB debugging is enabled on your Android device.)*

---

## 🛠 How to Build

If you want to reproduce this build yourself using the official binaries:

### Prerequisites
* `wget`
* `appimagetool` (downloaded during the process)

### Build Steps

1.  **Prepare the Directory**:
    ```bash
    mkdir Scrcpy.AppDir
    # Copy your scrcpy binaries (adb, scrcpy, scrcpy-server, etc.) into Scrcpy.AppDir/
    ```

2.  **Create Metadata**:
    Inside `Scrcpy.AppDir`, create a `scrcpy.desktop` file:
    ```ini
    [Desktop Entry]
    Name=scrcpy
    Type=Application
    Categories=Development;
    Terminal=false
    Exec=scrcpy
    Icon=icon
    Comment=Display and control your Android device
    ```

3.  **Create Entry Point**:
    ```bash
    cd Scrcpy.AppDir
    ln -s scrcpy AppRun
    cd ..
    ```

4.  **Package**:
    ```bash
    # Download tool
    wget [https://github.com/AppImage/appimagetool/releases/download/continuous/appimagetool-x86_64.AppImage](https://github.com/AppImage/appimagetool/releases/download/continuous/appimagetool-x86_64.AppImage)
    chmod +x appimagetool-x86_64.AppImage

    # Build
    ./appimagetool-x86_64.AppImage Scrcpy.AppDir
    ```

---

## ⚖️ License

* **Scrcpy** is developed by [Genymobile](https://github.com/Genymobile) and is licensed under Apache 2.0.
* This repository only provides the packaging scripts/builds to facilitate usage on Linux distributions.
