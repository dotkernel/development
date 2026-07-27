# System Requirements for AlmaLinux 10 on WSL 2

## Summary

Check whether WSL 2 is already installed and, if not, enable the required Windows features (Hyper-V, Virtual Machine Platform, Windows Subsystem for Linux) and set WSL 2 as the default version.

> If you are not using WSL, you can jump straight to the [AlmaLinux 10 Setup page](https://docs.dotkernel.org/development/v2/setup/setup-packages/).

> This is step 1 of 3 in setting up your development environment: **System Requirements** (this page) → [Install AlmaLinux 10](installation.md) → [Setup Packages](setup-packages.md).

> All commands on this page are executed in `Windows Terminal`, on your Windows host - not inside AlmaLinux 10.

## Minimum requirements

Before installing WSL 2, make sure your machine meets the following:

* Windows 10 version 1903 (build 18362) or higher, or Windows 11.
* A 64-bit processor with Second Level Address Translation (SLAT) support.
* Hardware virtualization enabled in your system's BIOS/UEFI (sometimes called `Intel VT-x`, `AMD-V`, or `SVM Mode`).
* At least 4 GB of RAM (8 GB or more recommended for running a full development stack comfortably).
* A few GB of free disk space for the distro plus additional space for your projects and databases.

For the full, up-to-date list, see Microsoft's [prerequisites documentation](https://learn.microsoft.com/en-us/windows/wsl/install#prerequisites).

## Check your WSL version

Open `Windows Terminal` and execute the following command:

```shell
wsl -v
```

The output should look similar to this (your version numbers will likely differ):

```text
WSL version: 2.2.4.0
Kernel version: 5.15.153.1-2
WSLg version: 1.0.61
MSRDC version: 1.2.5326
Direct3D version: 1.611.1-81528511
DXCore version: 10.0.26091.1-240325-1447.ge-release
Windows version: 10.0.22631.3737
```

If the output starts with `WSL version: 2.x.x.x`, you are ready to use WSL 2 and can proceed to [install AlmaLinux 10](installation.md).
If it doesn't (for example, it shows `WSL version: 1.x.x.x`, or the command isn't recognized), continue with the section below.

## Install WSL 2

Once you've confirmed your machine meets the [minimum requirements](#minimum-requirements) above, open the `Run` prompt by pressing `Win` + `r`, type `OptionalFeatures` in the dialog and press `Enter`.
This will open a window where you can turn Windows features on/off.
Make sure that the below features are activated (checked):

* `Hyper-V` (including its sub-features)
* `Virtual Machine Platform`
* `Windows Subsystem for Linux`

> If any of the above features are missing, then first you need to install them manually using [this guide](https://docs.microsoft.com/en-us/windows/wsl/install-manual) and then continue with the below steps.

Click `Ok` and restart your computer.

Open Microsoft Store, search for `Windows Subsystem for Linux` and install it.

Make sure that version `2` of WSL is set as default by executing the below command in Windows Terminal:

```shell
wsl --set-default-version 2
```

Run `wsl -v` again - this time the output should display `WSL version: 2.x.x.x` (in the same format shown [above](#check-your-wsl-version)), which means that your system is ready for using WSL 2 and you can proceed to [install AlmaLinux 10](installation.md).

## FAQ

### What if `wsl -v` shows WSL version 1 instead of 2?

Run:

```shell
wsl --set-default-version 2
```

Then re-run `wsl -v` to confirm the version has switched.

### What if the `OptionalFeatures` dialog doesn't show all required features?

Install them manually first using Microsoft's [manual install guide](https://docs.microsoft.com/en-us/windows/wsl/install-manual), then continue with the steps above.

### Do I need to restart my computer after enabling the Windows features?

Yes, a restart is required after clicking `Ok` in the `OptionalFeatures` dialog for the feature changes to take effect.

### What if my machine doesn't meet the minimum requirements for WSL 2?

Double-check the [minimum requirements](#minimum-requirements) above, in particular that hardware virtualization is enabled in your BIOS/UEFI - this is the most common blocker.
If you're still unsure, consult Microsoft's [prerequisites documentation](https://learn.microsoft.com/en-us/windows/wsl/install#prerequisites).
