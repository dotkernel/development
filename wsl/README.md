## Install WSL development environment

[< Back](../README.md)


## Prerequisites
Before proceeding to the installation, we need to make sure your machine is ready for running WSL2.

Open the `Run` prompt by pressing `Win`+`r` and type in the dialog `OptionalFeatures`, then press `Enter`.

This will open a window where you can turn Windows features on/off.

Make sure the next features are activated (checked):
* `Hyper-V` (including its sub-features)
* `Virtual Machine Platform`
* `Windows Subsystem for Linux`

> If any of the above features is missing, then first you need to install them manually using [this guide](https://docs.microsoft.com/en-us/windows/wsl/install-manual) and then continue with the below steps.

Click `Ok` and restart your computer.

Make sure WSL2 is set ad default by executing the below command in Windows Terminal:

    wsl --set-default-version 2


## Choose the Operating System you wish to use for development
* [Ubuntu 20](os/ubuntu20/README.md)
* [Alma Linux 8](os/almalinux8/README.md)
