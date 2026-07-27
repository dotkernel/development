---
title: System Requirements for AlmaLinux 10 on WSL 2
description: Check whether WSL 2 is already installed, enable the required Windows features (Hyper-V, Virtual Machine Platform, Windows Subsystem for Linux), and set WSL 2 as the default version.
author: "admin"
date_published: "2026-07-27"
canonical_url: "https://docs.dotkernel.org/development/v2/setup/system-requirements"
category: "Development"
language: "en"
---

# System Requirements for AlmaLinux 10 on WSL 2

## TL;DR

Check whether WSL 2 is already installed and, if not, enable the required Windows features (Hyper-V, Virtual Machine Platform, Windows Subsystem for Linux) and set WSL 2 as the default version. All commands run in `Windows Terminal` on the Windows host, not inside AlmaLinux 10. This is step 1 of 3, before [Install AlmaLinux 10](installation.md) and [Setup Packages](setup-packages.md).

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
