# Install AlmaLinux 10 on WSL 2

## Summary

Stop any other running WSL 2 distros, then install the AlmaLinux 10 distro (`wsl --install -d AlmaLinux-10`) and create the initial Unix user account.

> If you are not using WSL, you can jump straight to the [AlmaLinux 10 Setup page](https://docs.dotkernel.org/development/v2/setup/setup-packages/).

> This is step 2 of 3: [System Requirements](system-requirements.md) → **Install AlmaLinux 10** (this page) → [Setup Packages](setup-packages.md).

> All commands on this page are executed in `Windows Terminal`, on your Windows host — not inside AlmaLinux 10.

Before proceeding with the installation, we need to make sure that no other WSL 2 distribution (aka: _distro_) is running.
This is important because this installation will fail if required ports are already in use by another distro.

> The distro is downloaded over the internet, so make sure you have a stable connection. Depending on your connection speed, the download can take anywhere from a couple of minutes to significantly longer.

Open `Windows Terminal`.

## Stop other WSL 2 distros

List all installed distros:

```shell
wsl -l -v
```

If there is no other distro installed, you will see the below output (an empty list):

```text
  NAME            STATE           VERSION
```

In this case, you can jump to the [installation](#install-almalinux-10) section.

If you have other distros installed, the output could look similar to the below:

```text
  NAME            STATE           VERSION
  AlmaLinux-8     Stopped         2
* AlmaLinux-9     Running         2
```

Make sure that the **STATE** column reads **Stopped** for all distros.
If any of them reads **Running**, you must stop it first by executing `wsl -t <distro-name>` after replacing `<distro-name>` with the name of the distro you want to stop.
Once you have stopped all distros, you can continue to the [installation](#install-almalinux-10) section.

## Install AlmaLinux 10

List the available Linux distros by executing:

```shell
wsl --list --online
```

Depending on the list of distros available at the moment you run the command, the output should look similar to the below:

```text
The following is a list of valid distributions that can be installed.
Install using 'wsl.exe --install <Distro>'.

NAME                            FRIENDLY NAME
AlmaLinux-8                     AlmaLinux OS 8
AlmaLinux-9                     AlmaLinux OS 9
AlmaLinux-Kitten-10             AlmaLinux OS Kitten 10
AlmaLinux-10                    AlmaLinux OS 10
Debian                          Debian GNU/Linux
FedoraLinux-42                  Fedora Linux 42
SUSE-Linux-Enterprise-15-SP5    SUSE Linux Enterprise 15 SP5
SUSE-Linux-Enterprise-15-SP6    SUSE Linux Enterprise 15 SP6
Ubuntu                          Ubuntu
Ubuntu-24.04                    Ubuntu 24.04 LTS
archlinux                       Arch Linux
kali-linux                      Kali Linux Rolling
openSUSE-Tumbleweed             openSUSE Tumbleweed
openSUSE-Leap-15.6              openSUSE Leap 15.6
Ubuntu-18.04                    Ubuntu 18.04 LTS
Ubuntu-20.04                    Ubuntu 20.04 LTS
Ubuntu-22.04                    Ubuntu 22.04 LTS
OracleLinux_7_9                 Oracle Linux 7.9
OracleLinux_8_7                 Oracle Linux 8.7
OracleLinux_9_1                 Oracle Linux 9.1
```

Note the two columns: **NAME** and **FRIENDLY NAME**.
To install a specific distro, use the value from the **NAME** column, in this case: `AlmaLinux-10`.

> If you try to install a distro that is already installed, the installation process will fail:

```text
Downloading: AlmaLinux OS 10
Installing: AlmaLinux OS 10
A distribution with the supplied name already exists. Use --name to choose a different name.
Error code: Wsl/InstallDistro/Service/RegisterDistro/ERROR_ALREADY_EXISTS
```

Install the **AlmaLinux 10** distro by executing the below command:

```shell
wsl --install -d AlmaLinux-10
```

You should see the download progress—once finished, the output should look like this:

```text
Downloading: AlmaLinux OS 10
Installing: AlmaLinux OS 10
Distribution successfully installed. It can be launched via 'wsl.exe -d AlmaLinux-10'
Launching AlmaLinux-10...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username:
```

As per the last line, the installation process now prompts you to enter a username.
This is the username you will use inside **AlmaLinux 10**, and it can be any alphanumeric string (for example `dotkernel`):

Next, you are prompted to change the password associated with your chosen username (you will not see what you are typing, that's a security measure in Linux regarding passwords):

```shell
Enter new UNIX username: dotkernel.
New password:
```

Depending on the strength of your password, you might get a `BAD PASSWORD: <some-reason>` message (if you want to choose a different password, hit `Enter` and you are taken back to the previous step—else, continue with retyping your password):

Next, you are asked to retype your password:

```text
Retype new password:
```

Finally, you should see the following message:

```text
passwd: password updated successfully
[<your-alma-linux-10-username>@<your-device-name> <your-windows-username>]$
```

> At this point your terminal has dropped you inside the **AlmaLinux 10** shell (notice the prompt changed). Keep this window open and continue directly with [Setup Packages](setup-packages.md) — its commands run inside AlmaLinux 10, not in Windows Terminal. If you close this window, see [Running on WSL 2](../running.md) to reconnect.

## Next step

Continue to [Setup Packages](setup-packages.md) to install the required system packages and provision your development environment.

## FAQ

### What if the installation fails because a distro is already running?

Make sure no other WSL 2 distro is running by checking `wsl -l -v` — all distros must show **Stopped**. Stop any running distro with `wsl -t <distro-name>` before retrying the installation.

### What if `AlmaLinux-10` is already installed?

The installation will fail with `A distribution with the supplied name already exists`. Use a different `--name` value, or remove the existing distro first if you intend to reinstall it.

### Does the username need to match my Windows username?

No, the Unix username created during installation can be any alphanumeric string and does not need to match your Windows username.

### What if I get a `BAD PASSWORD` message?

This is just a strength warning. Press `Enter` to choose a different password, or continue retyping the same one if you want to keep it.

### Where can I find the list of available distros if `AlmaLinux-10` isn't shown?

Run `wsl --list --online` to see the current list of installable distros and their **NAME** values.

### What if `wsl --install -d AlmaLinux-10` fails or hangs with no clear error?

This is most often caused by hardware virtualization being disabled in your BIOS/UEFI, or by Hyper-V/Virtual Machine Platform not being enabled — revisit the [System Requirements](system-requirements.md) page and confirm both. A blocked or unstable internet connection (including corporate proxies/firewalls) can also cause the download to stall.

### What if the Microsoft Store is unavailable or blocked (for example, on a work laptop)?

You don't need the Store for this step — `wsl --install -d AlmaLinux-10` downloads and registers the distro directly, without going through the Store.

### How do I completely remove AlmaLinux-10 and start over?

Unregister the distro, which deletes it and all its data, then reinstall it from scratch:

```shell
wsl --unregister AlmaLinux-10
```

Once unregistered, you can run `wsl --install -d AlmaLinux-10` again as described above.
