# How to Set Up WSL2 on Windows 11

Windows 11 ships with WSL2 support built in — no manual kernel updates, no legacy workarounds. This is the cleanest path to a Linux environment on Windows I've seen so far.

---

## Prerequisites

- Windows 11 (any edition — Home, Pro, Enterprise)
- A user account with **Administrator** privileges
- Internet connection for downloading the distro

---

## Step 1 — Open PowerShell as Administrator

Right-click the Start button → **Terminal (Admin)** or search for **PowerShell**, right-click → **Run as administrator**.

You'll need this for the install command to work.

---

## Step 2 — Install WSL2

One command does everything — enables the WSL feature, sets version 2 as default, and installs Ubuntu as the default distro:

```powershell
wsl --install
```

Output you'll see:

```
Installing: Virtual Machine Platform
Virtual Machine Platform has been installed.
Installing: Windows Subsystem for Linux
Windows Subsystem for Linux has been installed.
Downloading: WSL Kernel
Installing: WSL Kernel
WSL Kernel has been installed.
Downloading: Ubuntu
The requested operation is successful.
```

> If you want a specific distro instead of Ubuntu, use `wsl --install -d <DistroName>`. Run `wsl --list --online` to see what's available.

---

## Step 3 — Restart Your Machine

WSL2 requires a reboot to finish the setup. Save anything open and restart.

```powershell
Restart-Computer
```

Or just restart manually from the Start menu.

---

## Step 4 — Finish the Linux Setup

After reboot, Ubuntu (or whichever distro you picked) will launch automatically and ask you to create a user account:

```
Enter new UNIX username: localadmin
New password:
Retype new password:
passwd: password updated successfully
```

Pick a username and a password. The password won't show as you type — that's normal.

Once done you'll land at the shell prompt:

```
localadmin@MACHINE-NAME:~$
```

You're in.

---

## Step 5 — Verify WSL2 is Running

Open a new PowerShell window and check:

```powershell
wsl -l -v
```

Expected output:

```
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

The `VERSION 2` confirms you're on WSL2, not the older WSL1. The `*` marks your default distro.

---

## Step 6 — Update the Distro

First thing after a fresh install — update the package list:

```bash
sudo apt update && sudo apt upgrade -y
```

Takes a minute or two. Do it now and you won't have to think about it again for a while.

---

## Step 7 — Install Windows Terminal (if you haven't already)

WSL2 works fine in the default console but Windows Terminal is significantly better — tabs, split panes, profiles per distro, proper font rendering.

Install it from the Microsoft Store or:

```powershell
winget install Microsoft.WindowsTerminal
```

Once installed, open it and your Linux distro will already be listed as a profile in the `+` dropdown.

---

## Useful Commands to Know

| Command | What it does |
|---|---|
| `wsl --install` | Install WSL2 with default distro (Ubuntu) |
| `wsl --install -d Debian` | Install a specific distro |
| `wsl --list --online` | See all available distros |
| `wsl -l -v` | List installed distros with version and state |
| `wsl --set-default-version 2` | Force WSL2 for all future instros |
| `wsl --shutdown` | Stop all running WSL instances |
| `wsl --update` | Update the WSL kernel |
| `wsl -d Ubuntu` | Launch a specific distro |

---

## Accessing Files Between Windows and Linux

From inside WSL2, your Windows drives are mounted under `/mnt/`:

```bash
ls /mnt/c/Users/
```

From Windows Explorer, type this in the address bar to browse your Linux filesystem:

```
\\wsl$\Ubuntu
```

Or just run `explorer.exe .` from inside your WSL2 terminal to open the current folder in Explorer.

---

## Troubleshooting

**`wsl --install` says WSL is already installed but nothing works**
- Run `wsl --update` to make sure the kernel is current
- Then `wsl --shutdown` and try again

**Stuck on "Installing" after reboot**
- Open the Microsoft Store and check for pending updates — the Ubuntu app sometimes needs to finish installing from there

**`Error: 0x80370102` — virtualization not enabled**
- Reboot into BIOS/UEFI and enable **Intel VT-x** or **AMD-V** (virtualization)
- On most machines it's under Advanced → CPU Configuration

**WSL1 instead of WSL2 showing in `wsl -l -v`**
- Set the default version: `wsl --set-default-version 2`
- Convert an existing distro: `wsl --set-version Ubuntu 2`

---

## What's Next

- [Install a Debian instance alongside Ubuntu](./2%20-%20provision-debian-wsl2.md)
- [Set up Git with SSH keys in WSL2](3%20-%20git-ssh-setup.md)

---

*Tested on Windows 11 23H2. If something looks different on your machine, the WSL docs at [aka.ms/wsl](https://aka.ms/wsl) are actually pretty good.*