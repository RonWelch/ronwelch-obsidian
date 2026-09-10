---
title: Wiki/WSL/Index.md at main
source: https://gldas05302.goldlnk.rootlnka.net/gitea/agentic-dev/Wiki/src/branch/main/WSL/Index.md
author:
  - "[[agentic-dev]]"
published:
created: 2026-09-04
description: Wiki - A collection of Wiki's for stuff that _might_ be related to Agentic Development.
tags:
  - clippings
  - WSL
  - BAE
---
## WSL at BAE

Windows Subsystem for Linux (WSL) can be useful to stand up dev environments using Docker or for testing out builds on Linux. It's trickier to install WSL here than it is elsewhere, but it can be done. I've had success using these steps.

## Step 1: Enable the WSL Windows Feature

You must first enable the "Windows Subsystem for Linux" optional feature before installing any Linux distributions on Windows.

Open PowerShell as Administrator (using Run Elevated) and enter this command:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

## Step 2: Enable the Virtual Machine Windows Feature

Before installing WSL 2, you must enable the Virtual Machine Platform optional feature. Your machine will require virtualization capabilities to use this feature.

Open PowerShell as Administrator (again using Run Elevated) and run:

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

## Step 3: Download and Install the Latest WSL Release

Note: This works to upgrade the WSL too.

Navigate to the [Github Releases Webpage for WSL](https://github.com/microsoft/WSL/releases) and download the latest (stable) release. WSL is now an open source project with a fair amount of engagement. At the time of this writing, the most current stable release is 2.6.1, but this is likely change to since stable builds are being released about every other month.

Once the release has been downloaded, right click to "Run Elevated". The installer will run but *may end silently*. To determine if WSL has been installed issue `wsl --version` in PowerShell. The output should look something like this,

```powershell
C:\Users\michael.atkisson> wsl --version
WSL version: 2.6.1.0
Kernel version: 6.6.87.2-1
WSLg version: 1.0.66
MSRDC version: 1.2.6353
Direct3D version: 1.611.1-81528511
DXCore version: 10.0.26100.1-240331-1435.ge-release
Windows version: 10.0.22631.5909
```

If you see a scrolling menu, WSL was not installed.

## Step 4: Install a Linux Distribution

First, determine which Distros are available for install with `wsl --list --online`.

```powershell
C:\Users\michael.atkisson> wsl --list --online
The following is a list of valid distributions that can be installed.
Install using 'wsl.exe --install <Distro>'.

NAME                            FRIENDLY NAME
AlmaLinux-8                     AlmaLinux OS 8
AlmaLinux-9                     AlmaLinux OS 9
Debian                          Debian GNU/Linux
Ubuntu                          Ubuntu
... (many more) ...
```

My advice is to install Debian, but Ubuntu is also a good choice. The rest of these instructions *assume you've installed Debian*.

In a **non-privileged** PowerShell console issue `wsl --install -d <Distro Name>`. This can take some time if your Internet connection is slow.

```powershell
C:\Users\michael.atkisson> wsl --install -d Debian
Downloading: Debian GNU/Linux
Installing: Debian GNU/Linux
Distribution successfully installed. It can be launched via 'wsl.exe -d Debian'
Launching Debian...
```

You'll be prompted to enter a username for the non-privileged Linux account. The name you choose is mostly arbitrary. I use the name `mike`, for example.

```powershell
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username: mike
New password:
Retype new password:
passwd: password updated successfully
usermod: no changes
```

## Step 5: Disable Hyper-V Firewall

Navigate to `Start > Search for WSL Settings > In Settings go to Networking` and set **Hyper-V Firewall enabled** to `Off`.

[![image.png](https://gldas05302.goldlnk.rootlnka.net/gitea/agentic-dev/Wiki/media/branch/main/WSL/resources/image-3351a6d3-2328-41df-9f0c-3a194acc0ff2.png)](https://gldas05302.goldlnk.rootlnka.net/gitea/agentic-dev/Wiki/src/branch/main/WSL/resources/image-3351a6d3-2328-41df-9f0c-3a194acc0ff2.png)

## Step 6: Update the Linux Distro

In PowerShell (non-privileged) issue `wsl` and, in Debian issue, `sudo apt update` followed by `sudo apt upgrade`.

[![image.png](https://gldas05302.goldlnk.rootlnka.net/gitea/agentic-dev/Wiki/media/branch/main/WSL/resources/image-77b6c07f-189f-4daf-8d54-66b69d7f0dbc.png)](https://gldas05302.goldlnk.rootlnka.net/gitea/agentic-dev/Wiki/src/branch/main/WSL/resources/image-77b6c07f-189f-4daf-8d54-66b69d7f0dbc.png)

## Step 7 (optional, but recommended): Install Supporting Software

In my travels, I've found the following software to be helpful, YMMV.

Here's a quick way to install all of these at once on Debian,

```bash
sudo apt install -y build-essential curl dos2unix git wget gnupg lsb-release icu-devtools
```

## Additional Topics

- [WSL Basics](https://gldas05302.goldlnk.rootlnka.net/gitea/agentic-dev/Wiki/src/branch/main/WSL/WSL-Basics.md)
- [Web Traffic](https://gldas05302.goldlnk.rootlnka.net/gitea/agentic-dev/Wiki/src/branch/main/WSL/Web-Traffic.md)
- [Git Credential Manager](https://gldas05302.goldlnk.rootlnka.net/gitea/agentic-dev/Wiki/src/branch/main/WSL/Git-Credential-Manager.md)
- [Installing.NET](https://gldas05302.goldlnk.rootlnka.net/gitea/agentic-dev/Wiki/src/branch/main/WSL/Installing-.NET.md)
- [Installing Docker](https://gldas05302.goldlnk.rootlnka.net/gitea/agentic-dev/Wiki/src/branch/main/WSL/Installing-Docker.md)
- [Installing Rust](https://gldas05302.goldlnk.rootlnka.net/gitea/agentic-dev/Wiki/src/branch/main/WSL/Installing-Rust.md)