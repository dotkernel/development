# Install AlmaLinux10

Open `Windows Terminal`.

List the available Linux distributions (aka: _distros_) by executing:

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
Install the AlmaLinux10 distro by executing the below command:

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
This is the username you will use inside AlmaLinux10, and it can be any alphanumeric string (for example `dotkernel`):

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
[<your-almalinux10-username>@<your-device-name> <your-windows-username>]$
```
