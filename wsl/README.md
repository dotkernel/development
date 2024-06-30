# Install WSL development environment

[< Back](../README.md)

## Prerequisites

Before proceeding with the installation, please consult Microsoft's
[documentation](https://learn.microsoft.com/en-us/windows/wsl/install#prerequisites) regarding the minimum requirements
for running WSL2.

Once you know that your machine can run WSL2, open the `Run` prompt by pressing `Win`+`r` and type in the dialog
`OptionalFeatures`, then press `Enter`.

This will open a window where you can turn Windows features on/off.

Make sure the next features are activated (checked):

- `Hyper-V` (including its sub-features)
- `Virtual Machine Platform`
- `Windows Subsystem for Linux`

> If any of the above features is missing, then first you need to install them manually using
> [this guide](https://docs.microsoft.com/en-us/windows/wsl/install-manual) and then continue with the below steps.

Click `Ok` and restart your computer.

Open Microsoft Store, search for `Windows Subsystem for Linux` and install it.

Make sure WSL2 is set as default by executing the below command in Windows Terminal:

```shell
wsl --set-default-version 2
```

## Choose the Operating System you want to use for development:

- [Ubuntu 20](os/ubuntu20/README.md)
- [AlmaLinux 9](os/almalinux9/README.md)
