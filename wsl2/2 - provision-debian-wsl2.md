# How to Provision a New Debian Instance in WSL2

> **WSL2** (Windows Subsystem for Linux 2) lets you run a real Linux environment directly on Windows — no VM, no dual boot. In this guide, you'll install a fresh **Debian GNU/Linux** instance in just a few minutes.

---

## Prerequisites

- Windows 10 (version 2004+) or Windows 11
- WSL2 already enabled on your machine
- **Windows Terminal** (recommended for a better experience) — available free from the Microsoft Store
- PowerShell running as **Administrator**

---

## Step 1 — Check Available Distributions

Before installing, let's see which Linux distributions are officially available:

```powershell
wsl --list --online
```

You'll get a table like this:

```
NAME                            FRIENDLY NAME
Ubuntu                          Ubuntu
Ubuntu-24.04                    Ubuntu 24.04 LTS
Ubuntu-22.04                    Ubuntu 22.04 LTS
Debian                          Debian GNU/Linux
kali-linux                      Kali Linux Rolling
archlinux                       Arch Linux
...
```

> There are 20+ distributions available, including Ubuntu, Debian, Kali, Fedora, openSUSE, AlmaLinux, and more.

---

## Step 2 — Install Debian

Run the following command to install Debian:

```powershell
wsl --install -d Debian
```

WSL2 will download and install Debian GNU/Linux automatically:

```
Downloading: Debian GNU/Linux
Installing: Debian GNU/Linux
Distribution successfully installed. It can be launched via 'wsl.exe -d Debian'
Launching Debian...
```

---

## Step 3 — Create Your UNIX User Account

Once Debian launches, it will prompt you to create a default UNIX user. This username **does not need to match** your Windows username.

```
Please create a default UNIX user account.
Enter new UNIX username: localadmin
New password:
Retype new password:
passwd: password updated successfully
usermod: no changes
localadmin@WINAA5CD308C22C:/mnt/c/WINDOWS/system32$
```

### ⚠️ Common Gotcha: Password Mismatch

If you type mismatching passwords, WSL will warn you and let you retry:

```
Sorry, passwords do not match.
passwd: Authentication token manipulation error
passwd: password unchanged
warn: '/bin/passwd localadmin' failed with status 10. Continuing.
warn: wrong password given or password retyped incorrectly
Try again? [y/N] y
```

Just type `y` and re-enter your password carefully. Once successful, you'll land at the Debian shell prompt.

---

## Step 4 — Verify the Installation

Back in PowerShell, confirm your new Debian instance is running:

```powershell
wsl -l -v
```

Expected output:

```
  NAME               STATE           VERSION
* Ubuntu-22.04       Running         2
  generic-serverless Stopped         2
  Debian             Running         2
```

Your Debian instance is listed with **VERSION 2** (WSL2) and **STATE: Running**. ✅

---

## Step 5 — Launch Debian

You can launch it directly from PowerShell at any time:

```powershell
wsl -d Debian
```

This drops you straight into the Debian shell:

```
localadmin@WINAA5CD308C22C:/mnt/c/WINDOWS/system32$
```

---

## Bonus — Launch from Windows Terminal

If you're using **Windows Terminal**, Debian is automatically added as a profile. Click the **`+` dropdown** in the tab bar and you'll see **Debian** listed alongside your other shells.

You can also use the keyboard shortcut shown in the dropdown (e.g., `Ctrl+Shift+8`) to open it directly.

Debian opens in its own tab with a dedicated prompt:

```
localadmin@localhost:~$
```

---

## Quick Reference

| Command | Purpose |
|---|---|
| `wsl --list --online` | See all available distributions |
| `wsl --install -d Debian` | Install Debian |
| `wsl -l -v` | List installed distros with state & version |
| `wsl -d Debian` | Launch Debian |
| `wsl --shutdown` | Stop all running WSL instances |

---

## What's Next?

Now that your Debian instance is up and running, here are some good first steps:

- **Update packages:** `sudo apt update && sudo apt upgrade -y`
- **Install essentials:** `sudo apt install git curl wget build-essential git -y`
- **Set Debian as your default WSL distro:** `wsl --set-default Debian`
- **Access Windows files** from Debian at `/mnt/c/Users/<YourWindowsUsername>/`
- **Access Debian files** from Windows Explorer by typing `\\wsl$\Debian` in the address bar

---