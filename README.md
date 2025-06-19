# WSL2 development environment

This is a collection of Ansible scripts helping with the creation and maintenance of your WSL2 development environment.

If you're not already using it, we recommend you to install [Windows Terminal](https://apps.microsoft.com/detail/9n0dx20hk701?hl=en-US&gl=US).
It is a modern tool that incorporates the power of multiple already known command-line applications like `Windows PowerShell`, `Linux shell`, and more...

## Check if WLS2 is already available

Open `Windows Terminal` and execute the following command:

```shell
wsl -v
```

The output should look similar to this:

```text
WSL version: 2.2.4.0
Kernel version: 5.15.153.1-2
WSLg version: 1.0.61
MSRDC version: 1.2.5326
Direct3D version: 1.611.1-81528511
DXCore version: 10.0.26091.1-240325-1447.ge-release
Windows version: 10.0.22631.3737
```

If the output starts with `WSL version: 2.x.x.x`, you are ready to use **WSL2** and can proceed to [install AlmaLinux 10](wsl/README.md).

## Install WSL2

Before proceeding with the installation, please consult Microsoft's [documentation](https://learn.microsoft.com/en-us/windows/wsl/install#prerequisites) regarding the minimum requirements for running WSL2.

Once you identified that your machine can run WSL2, open the `Run` prompt by pressing `Win` + `r`, type `OptionalFeatures` in the dialog and press `Enter`.
This will open a window where you can turn Windows features on/off.
Make sure that the below features are activated (checked):

* `Hyper-V` (including its sub-features)
* `Virtual Machine Platform`
* `Windows Subsystem for Linux`

> If any of the above features are missing, then first you need to install them manually using [this guide](https://docs.microsoft.com/en-us/windows/wsl/install-manual) and then continue with the below steps.

Click `Ok` and restart your computer.

Open Microsoft Store, search for `Windows Subsystem for Linux` and install it.

Make sure that version **2** of WSL is set as default by executing the below command in Windows Terminal:

```shell
wsl --set-default-version 2
```

To test, run again the following command:

```shell
wsl -v
```

This time the output should display `WSL version: 2.x.x.x`, which means that your system is ready for using **WSL2** and you can proceed to [install AlmaLinux 10](wsl/README.md).
