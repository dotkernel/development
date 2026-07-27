---
title: Install AlmaLinux 10 on WSL 2
description: Learn how to install AlmaLinux 10 on WSL 2 - stop any other running distros, run wsl --install -d AlmaLinux-10, and create your initial Unix user account.
author: "admin"
date_published: "2026-07-27"
canonical_url: "https://docs.dotkernel.org/development/v2/setup/installation"
category: "Development"
language: "en"
---

# Install AlmaLinux 10 on WSL 2

## TL;DR

Stop any other running WSL 2 distros, then install the AlmaLinux 10 distro (`wsl --install -d AlmaLinux-10`) and create the initial Unix user account. All commands run in `Windows Terminal` on the Windows host, not inside AlmaLinux 10. This is step 2 of 3, after [System Requirements](system-requirements.md) and before [Setup Packages](setup-packages.md).

## FAQ

### What if the installation fails because a distro is already running?

Make sure no other WSL 2 distro is running by checking:

```shell
wsl -l -v
```

All distros must show **Stopped**. Stop any running distro with the below command before retrying the installation:

```shell
wsl -t <distro-name>
```

### What if `AlmaLinux-10` is already installed?

The installation will fail with `A distribution with the supplied name already exists`.
Use a different `--name` value, or remove the existing distro first if you intend to reinstall it.

### Does the username need to match my Windows username?

No, the Unix username created during installation can be any alphanumeric string and does not need to match your Windows username.

### What if I get a `BAD PASSWORD` message?

This is just a strength warning.
Press `Enter` to choose a different password, or continue retyping the same one if you want to keep it.

### Where can I find the list of available distros if `AlmaLinux-10` isn't shown?

Run the below command to see the current list of installable distros and their **NAME** values:

```shell
wsl --list --online
```

### What if `wsl --install -d AlmaLinux-10` fails or hangs with no clear error?

This is most often caused by hardware virtualization being disabled in your BIOS/UEFI, or by Hyper-V/Virtual Machine Platform not being enabled.
Revisit the [System Requirements](system-requirements.md) page and confirm both.
A blocked or unstable internet connection (including corporate proxies/firewalls) can also cause the download to stall.

### What if the Microsoft Store is unavailable or blocked (for example, on a work laptop)?

You don't need the Store for this step.
`wsl --install -d AlmaLinux-10` downloads and registers the distro directly, without going through the Store.

### How do I completely remove AlmaLinux-10 and start over?

Unregister the distro, which deletes it and all its data, then reinstall it from scratch:

```shell
wsl --unregister AlmaLinux-10
```

Once unregistered, you can run `wsl --install -d AlmaLinux-10` again as described above.
