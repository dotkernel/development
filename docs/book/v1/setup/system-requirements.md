# System Requirements

First, you need to check if your system is ready for using **WSL2**. Open `Windows Terminal` and execute the following command:

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

If the output starts with `WSL version: 2.x`, you are ready to use **WSL2** and can continue with [the installation](installation.md).

Else, you need to install **WSL2** and its components as shown below.

Before proceeding with the installation, please consult Microsoft's [documentation](https://learn.microsoft.com/en-us/windows/wsl/install#prerequisites) regarding the minimum requirements for running **WSL2**.

Once you know that your system can run **WSL2**, open the `Run` prompt by pressing `Win`+`r` and type in the dialog `OptionalFeatures`, then press `Enter`.

This will open a window where you can turn Windows features on/off.

Make sure the next features are activated (checked):

* `Hyper-V` (including its sub-features)
* `Virtual Machine Platform`
* `Windows Subsystem for Linux`

> If any of the above features are missing, then first you need to install them manually using [this guide](https://docs.microsoft.com/en-us/windows/wsl/install-manual) and then continue with the below steps.

Click `Ok` and restart your computer.

Open Microsoft Store, search for `Windows Subsystem for Linux` and install it.

Make sure **WSL2** is set as default by executing the below command in `Windows Terminal`:

```shell
wsl --set-default-version 2
```

To test, run again the following command:

```shell
wsl -v
```

This time the output should display `WSL version: 2.x`, which means that your system is ready for using **WSL2** and you can continue with the [installation](installation.md).
