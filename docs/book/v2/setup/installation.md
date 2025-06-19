# AlmaLinux 10 Installation

Before proceeding with the installation, we need to make sure that no other WSL2 distribution (aka: _distro_) is running.
This is important because this installation will fail if required ports are already in use by another distro.

Open `Windows Terminal`.

## Stop other WSL2 distros

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
If any of them reads **Running**, you must stop if first by executing `wsl -t <distro-name>` after replacing `<distro-name>` with the name of the distro you want to stop.
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
