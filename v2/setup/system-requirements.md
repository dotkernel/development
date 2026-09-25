# System Requirements for AlmaLinux 10 on WSL 2

## Summary

Check whether WSL 2 is already installed and, if not, install it with a single command (`wsl --install --no-distribution`).

> If you are not using WSL, you can jump straight to the [Setup Packages page](setup-packages.md).

> This is step 1 of 3 in setting up your development environment: **System Requirements** (this page) → [Install AlmaLinux 10](installation.md) → [Setup Packages](setup-packages.md).

> All commands on this page are executed in `Windows Terminal`, on your Windows host - not inside AlmaLinux 10.

## Minimum requirements

Before installing WSL 2, make sure your machine meets the following:

* Windows 11.
* A 64-bit processor with Second Level Address Translation (SLAT) support.
* Hardware virtualization enabled in your system's BIOS/UEFI (sometimes called `Intel VT-x`, `AMD-V`, or `SVM Mode`).
* At least 4 GB of RAM (8 GB or more recommended for running a full development stack comfortably).
* A few GB of free disk space for the distro plus additional space for your projects and databases.

For the full, up-to-date list, see Microsoft's [prerequisites documentation](https://learn.microsoft.com/en-us/windows/wsl/install#prerequisites).

## Check your WSL version

Open `Windows Terminal` and execute the following command:

```shell
wsl --version
```

If the command is recognized, the output should look similar to this (your version numbers will likely differ):

```text
WSL version: 2.2.4.0
Kernel version: 5.15.153.1-2
WSLg version: 1.0.61
MSRDC version: 1.2.5326
Direct3D version: 1.611.1-81528511
DXCore version: 10.0.26091.1-240325-1447.ge-release
Windows version: 10.0.22631.3737
```

This means you already have a modern WSL 2 install, and you can proceed directly to [install AlmaLinux 10](installation.md).

If instead the command is **not recognized** (an error instead of the output above), your system has only the older, inbox WSL component, which predates the `--version` flag entirely - continue with the section below to install the modern version.

## Install WSL 2

Run the below command to install WSL 2 without a default Linux distribution (you'll install **AlmaLinux 10** specifically on the next page, so there's no need for a default one here):

```shell
wsl --install --no-distribution
```

This single command enables the required Windows features (`Virtual Machine Platform` and `Windows Subsystem for Linux`), downloads and installs the WSL 2 kernel, and sets WSL 2 as the default version.

Restart your computer if prompted.

Once restarted, confirm the install by running `wsl --version` again (see [Check your WSL version](#check-your-wsl-version) above), then continue to [Install AlmaLinux 10](installation.md).

### Manual installation (fallback)

If `wsl --install` is blocked or unavailable on your machine (for example, a locked-down corporate device where automatic feature-enabling is disabled by policy), you can enable the required Windows features manually instead.

Open the `Run` prompt by pressing `Win` + `r`, type `OptionalFeatures` in the dialog and press `Enter`.
This will open a window where you can turn Windows features on/off.
Make sure that the below features are activated (checked):

* `Virtual Machine Platform`
* `Windows Subsystem for Linux`

> If any of the above features are missing, then first you need to install them manually using [this guide](https://docs.microsoft.com/en-us/windows/wsl/install-manual) and then continue with the below steps.

Click `Ok` and restart your computer.

Open Microsoft Store, search for `Windows Subsystem for Linux` and install it.

Make sure that version `2` of WSL is set as default by executing the below command in Windows Terminal:

```shell
wsl --set-default-version 2
```

Run `wsl --version` again to confirm the modern component is now in place, then continue to [Install AlmaLinux 10](installation.md).

## FAQ

**Q: What if `wsl --version` isn't recognized at all?**

A: That means you only have the legacy, inbox WSL component, which predates this flag - it errors instead of printing a version number. Follow [Install WSL 2](#install-wsl-2) above; running `wsl --install --no-distribution` upgrades you to the modern version.

**Q: What if `wsl --install` fails or is blocked by policy?**

A: Use the [manual installation](#manual-installation-fallback) steps above instead.

**Q: What if the `OptionalFeatures` dialog doesn't show all required features?**

A: Install them manually first using Microsoft's [manual install guide](https://docs.microsoft.com/en-us/windows/wsl/install-manual), then continue with the steps above.

**Q: Do I need to restart my computer after enabling the Windows features?**

A: Yes, a restart is required after clicking `Ok` in the `OptionalFeatures` dialog (or after `wsl --install --no-distribution` prompts for one) for the feature changes to take effect.

**Q: What if my machine doesn't meet the minimum requirements for WSL 2?**

A: Double-check the [minimum requirements](#minimum-requirements) above, in particular that hardware virtualization is enabled in your BIOS/UEFI - this is the most common blocker.
If you're still unsure, consult Microsoft's [prerequisites documentation](https://learn.microsoft.com/en-us/windows/wsl/install#prerequisites).
