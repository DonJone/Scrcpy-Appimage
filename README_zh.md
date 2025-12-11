# Scrcpy AppImage (非官方构建)

![License](https://img.shields.io/github/license/Donjone/Scrcpy-AppImage)
![Version](https://img.shields.io/badge/scrcpy-v3.3.3-green)
![Platform](https://img.shields.io/badge/platform-Linux-blue)

这是 [Genymobile/scrcpy](https://github.com/Genymobile/scrcpy) 的非官方 AppImage 构建版本。

## 🧐 为什么要创建这个仓库？

创建此仓库是为了解决一个特定问题：通过系统包管理器（例如 Fedora/RHEL 上的 `dnf`）安装 `scrcpy` 时，可能会遇到 **屏幕显示为灰度/黑白** 的异常情况。

这个 AppImage 将官方预编译的二进制文件 (v3.3.3) 打包成便携格式。通过绕过导致渲染故障的特定系统库版本，此构建确保在受影响的系统上能 **正常显示全彩画面**。

---

## ⚠️ 关于依赖项的重要说明

**这是一个“轻量级” AppImage。** 与捆绑了所有依赖库的标准 AppImage 不同，此包仅封装了预编译的二进制文件。它依赖于你的系统中已安装的基础运行库。

为了确保其正常运行，你的系统通常需要安装以下组件：
* `android-tools` (ADB)
* `SDL2`
* `ffmpeg` (libavcodec/libavformat)

如果 AppImage 无法启动，请先通过你的包管理器安装标准的 scrcpy 依赖项（例如：`sudo dnf install android-tools SDL2 ffmpeg`）。

---

## 🚀 安装与使用

### 选项 1：使用 Gear Lever 管理（推荐）

此 AppImage 完全兼容 [Gear Lever](https://flathub.org/apps/it.mijorus.gearlever)，这是 Linux 上一款流行的 AppImage 管理工具。它可以让你轻松地将 AppImage 集成到应用菜单中并管理更新。

1.  从 [Releases](../../releases) 页面 **下载** `.AppImage` 文件。
2.  打开 **Gear Lever**。
3.  点击 **"Open File" (打开文件)** 并选择 `Scrcpy-x86_64.AppImage`。
4.  Gear Lever 会自动将其移动到你的 AppImages 文件夹并创建桌面快捷方式。
5.  之后你可以直接从系统的应用启动器中运行 `scrcpy`。

### 选项 2：手动运行

1.  **下载** 最新的 `.AppImage` 文件。
2.  **赋予执行权限**：
    ```bash
    chmod +x Scrcpy-x86_64.AppImage
    ```
3.  **运行**：
    ```bash
    ./Scrcpy-x86_64.AppImage
    ```

*(注意：请确保你的 Android 设备已开启 USB 调试模式。)*

---

## 🛠 如何构建

如果你想使用官方二进制文件自行重新构建此 AppImage，请遵循以下步骤：

### 前置条件
* `wget`
* `appimagetool` (构建过程中会自动下载)

### 构建步骤

1.  **准备目录**：
    ```bash
    mkdir Scrcpy.AppDir
    # 将你的 scrcpy 二进制文件 (adb, scrcpy, scrcpy-server 等) 复制到 Scrcpy.AppDir/ 中
    ```

2.  **创建元数据**：
    在 `Scrcpy.AppDir` 目录内，创建一个 `scrcpy.desktop` 文件：
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

3.  **创建入口点**：
    ```bash
    cd Scrcpy.AppDir
    ln -s scrcpy AppRun
    cd ..
    ```

4.  **打包**：
    ```bash
    # 下载工具
    wget [https://github.com/AppImage/appimagetool/releases/download/continuous/appimagetool-x86_64.AppImage](https://github.com/AppImage/appimagetool/releases/download/continuous/appimagetool-x86_64.AppImage)
    chmod +x appimagetool-x86_64.AppImage

    # 构建
    ./appimagetool-x86_64.AppImage Scrcpy.AppDir
    ```

---

## ⚖️ 许可证

* **Scrcpy** 由 [Genymobile](https://github.com/Genymobile) 开发，采用 Apache 2.0 许可证。
* 本仓库仅提供打包脚本/构建文件，以方便在 Linux 发行版上使用。
