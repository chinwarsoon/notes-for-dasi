# 🐧 Ubuntu on WSL: The Complete Setup Guide

This document serves as a master reference for setting up, maintaining, and developing within **Ubuntu** via the **Windows Subsystem for Linux (WSL)**.

---

## 🏗️ Architecture Overview
Ubuntu on WSL is a "distribution" (distro) of Linux that runs alongside Windows. It uses a real Linux kernel managed by Windows to give you a full terminal environment without the overhead of a traditional Virtual Machine.

- **The Kernel (Linux):** Manages your CPU, memory, and hardware.
- **The Distribution (Ubuntu):** The user interface and software ecosystem.

---

## 🚀 Installation & Initialization
To install or manage Ubuntu, use the following commands in **PowerShell (Admin)**:

* **Install:** `wsl --install -d Ubuntu`
* **Check Status:** `wsl -l -v` (Lists installed distros and their WSL version).
* **Terminate:** `wsl --terminate Ubuntu` (Kills the instance to save RAM).
* **Reset/Delete:** `wsl --unregister Ubuntu` *(Warning: This permanently deletes all files!)*

> **Note:** When setting your password in the Ubuntu terminal, the cursor will not move. This is normal security behavior.

---

## 🛠️ System Maintenance (The "Golden Duo")
Run these inside the **Ubuntu Terminal** regularly to keep your system secure and up to date.

| Command | Purpose |
| :--- | :--- |
| `sudo apt update` | Refreshes the local "shopping list" of available software. |
| `sudo apt upgrade -y` | Downloads and installs the actual updates. |
| `sudo apt autoremove` | Removes old, unused dependencies to save space. |

**Pro Shortcut:** `sudo apt update && sudo apt upgrade -y`

---

## 📁 The Cross-Platform Bridge
WSL allows Windows and Linux to "talk" to each other's files.

* **Access Windows from Linux:** Your C: drive is located at `/mnt/c/`.
    * *Example:* `cd /mnt/c/Users/YourName/Downloads`
* **Access Linux from Windows:** Use the network path `\\wsl$\Ubuntu\` in File Explorer.
* **The Shortcut:** Type `explorer.exe .` inside any Ubuntu folder to open it in Windows File Explorer.

---

## 💻 Development with VS Code
The best way to code is using the **WSL** extension by Microsoft.

1.  **Install Extension:** Search for "WSL" in the VS Code Marketplace within Windows.
2.  **Launch from Terminal:** Navigate to your project in Ubuntu and type:
    ```bash
    code .
    ```
3.  **Verify Connection:** Check the bottom-left corner of VS Code; it should display **WSL: Ubuntu**.

---

## 🐍 Testing the Setup (Python Hello World)
Verify your environment by running a script that identifies the OS:

1.  **Create folder:** `mkdir hello-linux && cd hello-linux`
2.  **Create file:** `touch hello.py`
3.  **Add Code:**
    ```python
    import platform
    print(f"Hello from {platform.system()}!")
    print(f"Distro: {platform.freedesktop_os_release()['PRETTY_NAME']}")
    ```
4.  **Run:** `python3 hello.py`

---

## ⚡ Quick Reference Command Palette
* **`pwd`**: Print Working Directory (Where am I?).
* **`ls -la`**: List all files including hidden ones.
* **`htop`**: Interactive system monitor (Install via `sudo apt install htop`).
* **`exit`**: Close the current Ubuntu terminal session.
* **`wsl --shutdown`**: (Run in PowerShell) Stops all Linux activity to free up Windows RAM.