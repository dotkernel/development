# WSL2

[< Install Ansible development environment](../README.md)


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

Make sure you use WSL2 by default by executing the below command in Windows Terminal:

    wsl --set-default-version 2


## Install
Get a list of available distros by executing `wsl -l -o` or `wsl --list --online`:

    The following is a list of valid distributions that can be installed.
    Install using 'wsl --install -d <Distro>'.
    
    NAME            FRIENDLY NAME
    Ubuntu          Ubuntu
    Debian          Debian GNU/Linux
    kali-linux      Kali Linux Rolling
    openSUSE-42     openSUSE Leap 42
    SLES-12         SUSE Linux Enterprise Server v12
    Ubuntu-16.04    Ubuntu 16.04 LTS
    Ubuntu-18.04    Ubuntu 18.04 LTS
    Ubuntu-20.04    Ubuntu 20.04 LTS

Pick any distro and install it using the below command:

    wsl --install -d {distro}

after you replace `{distro}` with the name of the selected distro - for example:

    wsl --install -d Ubuntu

Once completed, a new terminal will open, prompting you to:
* `Enter new UNIX username:` - enter the username you will use to log in to this machine
* `Enter new UNIX password:` - enter the password you will use in combination with the selected username (you will not see what you're typing, that's a security measure in Linux regarding passwords)
* `Retype new UNIX password:` - repeat your password

At the end, you should see the below output:

    passwd: password updated successfully
    Installation successful!

More on installing WSL and other distributions can be found [here](https://docs.microsoft.com/en-us/windows/wsl/install)

You can close the installation window that was opened during the installation process.


## Change hostname displayed in terminal (recommended)
When you have more than one distro installed, identifying which distro your terminal is connected to can become difficult.

In the below example we will modify the displayed hostname, by changing it to `ubuntu2004php74` - you can use any name you see fit.

[Access your distro](#access-your-distros) and go to your home directory. Usually **/home/username**.

Then, locate and open `.bashrc` using your preferred text editor.

Inside this file, replace all occurrences of the string: **u@\h** with **u@ubuntu2004php74**.

Save the file and close it.

Next time you log in to this distro, the terminal will reflect the machine's name.


## Allow multiple instances of same Linux distribution (optional)
Using the default installation method, once a distro is installed, you cannot install another instance of the same distro.

For example, if you already have an instance of `Ubuntu`, executing `wsl --install -d Ubuntu` will open the existing instance instead of installing a new one.

In order to allow multiple instances of the same distro, we need to move the distro's disk drive file from the [default location](#wsl-distro-disk-drive-default-locations) to a custom one.

In this example we will move distro called `Ubuntu` to drive `D` - you can choose any drive you want, but storing them on drive `C` might require admin privileges.

First, we backup the distro:
* open Windows Terminal
* go to drive `D` using the command `cd D:`
* create a directory to store distros, for example `WSLs` using the command `cd WSLs`
* stop all distros using the command: `wsl --shutdown`
* make sure that all distros are stopped by [listing installed distros](#list-installed-distros)
* export distro, using the command `wsl --export Ubuntu ubuntu-backup.tar`

Next, we delete the distro `Ubuntu`:
* execute command `wsl --unregister Ubuntu`
* make sure the distro is not installed by [listing installed distros](#list-installed-distros)

Finally, we restore the distro from the backup to the new location:
* execute command `wsl --import Ubuntu Ubuntu ubuntu-backup.tar`
* make sure the distro has been reinstalled by [listing installed distros](#list-installed-distros)

**Note**: You can also use this method to create forks of the same distro, each of them running different version of PHP.


## List installed distros
Open Windows Terminal and execute `wsl -l -v` or `wsl --list --verbose`.

Depending on the installed distros, the output of this command will look similar to the following:

| NAME         | STATE   | VERSION |
|--------------|---------|---------|
| Debian       | running | 2       |
| Ubuntu       | stopped | 2       |
| Ubuntu-16.04 | stopped | 2       |
| Ubuntu-18.04 | stopped | 2       |
| Ubuntu-20.04 | stopped | 2       |

Although the column names are self-explanatory, here's a few details on the columns:
* `NAME`: distro name - use this string when you need to replace `{distro}` with a value
* `STATE`: indicates distro status (`running` or `stopped`)
* `VERSION`: indicates if a distro runs on WSL1 or WSL2


## Access your distros
There are multiple ways for accessing a Linux distro:
* from Windows Terminal: use the arrow found in the tab bar (if you just installed a new distro, you need to restart the terminal for it to appear in the list)
* from Windows Terminal: type `wsl -d {distro}` (replace {distro} with your `distro`), then hit `Enter`
* from Windows Explorer: type `\\wsl$\{distro}` (replace {distro} with your `distro`) in the address bar, then hit `Enter` (once there, you can also pin the directory to Quick access)


## Start distro
Open Windows Terminal. If you don't know the name of the distro you want to start, [list installed distros](#list-installed-distros).

You can start it by executing the following command (replace {distro} with your `distro`):

    wsl -d {distro}


## Stop distro
Open Windows Terminal. If you don't know the name of the distro you want to stop, [list installed distros](#list-installed-distros).

You can stop it by executing the following command (replace {distro} with your `distro`):

    wsl -t {distro}

OR

    wsl --terminate {distro}


## Restart distro
Open Windows Terminal. If you don't know the name of the distro you want to restart, [list installed distros](#list-installed-distros).

First, [stop the distro](#stop-distro), then [start it](#start-distro).


## WSL and PhpStorm
Check out [this](https://www.jetbrains.com/help/phpstorm/how-to-use-wsl-development-environment-in-product.html) article on working with PhpStorm.


## Check identity
See below methods to identify the user you are logged in with:

### Method 1:
By looking at your terminal's prompt area, you should see something similar to this:

    username@hostname:~$

The part before the `@` character is you current user, so in this example the username is `username`.

### Method 2:
Using your terminal and being logged in to your `distro`, type in the following command:

    whoami

The output of this command is the username you are currently logged in with.


## Switch from root to your username
[Check your identity](#switch-from-root-to-your-username) and if it's _root_, then execute the following command (replace {username} with your `username`):

    su - {username}

[Check your identity](#switch-from-root-to-your-username) to make sure that your identity has been switched to the desired username.


## WSL distro disk drive default locations
Below you will find the disk drive file locations for the most common distros:
* `Debian (latest)`: %USERPROFILE%\AppData\Local\Packages\TheDebianProject.DebianGNULinux_76v4gfsz19hv4\LocalState
* `Ubuntu (latest)`: %USERPROFILE%\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState
* `Ubuntu 16.04`: %USERPROFILE%\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu16.04onWindows_79rhkp1fndgsc\LocalState
* `Ubuntu 18.04`: %USERPROFILE%\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu18.04onWindows_79rhkp1fndgsc\LocalState
* `Ubuntu 20.04`: %USERPROFILE%\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu20.04onWindows_79rhkp1fndgsc\LocalState


## Getting help
If you need help using WSL, type `wsl --help` and hit `Enter`. This will output all available options on how to use WSL:

    Copyright (c) Microsoft Corporation. All rights reserved.

    Usage: wsl.exe [Argument] [Options...] [CommandLine]
    
    Arguments for running Linux binaries:

    If no command line is provided, wsl.exe launches the default shell.

    --exec, -e <CommandLine>
        Execute the specified command without using the default Linux shell.

    --
        Pass the remaining command line as is.

    Options:
    --cd <Directory>
    Sets the specified directory as the current working directory.
    If ~ is used the Linux user's home path will be used. If the path begins
    with a / character, it will be interpreted as an absolute Linux path.
    Otherwise, the value must be an absolute Windows path.

    --distribution, -d <Distro>
        Run the specified distribution.

    --user, -u <UserName>
        Run as the specified user.

    --system
        Launches a shell for the system distribution.

    Arguments for managing Windows Subsystem for Linux:

    --help
        Display usage information.

    --install [Options]
        Install additional Windows Subsystem for Linux distributions.
        For a list of valid distributions, use 'wsl --list --online'.

        Options:
            --distribution, -d [Argument]
                Downloads and installs a distribution by name.

                Arguments:
                    A valid distribution name (not case sensitive).

                Examples:
                    wsl --install -d Ubuntu
                    wsl --install --distribution Debian

    --set-default-version <Version>
        Changes the default install version for new distributions.

    --shutdown
        Immediately terminates all running distributions and the WSL 2
        lightweight utility virtual machine.

    --status
        Show the status of Windows Subsystem for Linux.

    --update [Options]
        If no options are specified, the WSL 2 kernel will be updated
        to the latest version.

        Options:
            --rollback
                Revert to the previous version of the WSL 2 kernel.

    Arguments for managing distributions in Windows Subsystem for Linux:

    --export <Distro> <FileName>
        Exports the distribution to a tar file.
        The filename can be - for standard output.

    --import <Distro> <InstallLocation> <FileName> [Options]
        Imports the specified tar file as a new distribution.
        The filename can be - for standard input.

        Options:
            --version <Version>
                Specifies the version to use for the new distribution.

    --list, -l [Options]
        Lists distributions.

        Options:
            --all
                List all distributions, including distributions that are
                currently being installed or uninstalled.

            --running
                List only distributions that are currently running.

            --quiet, -q
                Only show distribution names.

            --verbose, -v
                Show detailed information about all distributions.

            --online, -o
                Displays a list of available distributions for install with 'wsl --install'.

    --set-default, -s <Distro>
        Sets the distribution as the default.

    --set-version <Distro> <Version>
        Changes the version of the specified distribution.

    --terminate, -t <Distro>
        Terminates the specified distribution.

    --unregister <Distro>
        Unregisters the distribution and deletes the root filesystem.

    --mount <Disk>
        Attaches and mounts a physical disk in all WSL2 distributions.

        Options:
            --bare
                Attach the disk to WSL2, but don't mount it.

            --type <Type>
                Filesystem to use when mounting a disk, if not specified defaults to ext4.

            --options <Options>
                Additional mount options.

            --partition <Index>
                Index of the partition to mount, if not specified defaults to the whole disk.

    --unmount [Disk]
        Unmounts and detaches a disk from all WSL2 distributions.
        Unmounts and detaches all disks if called without argument.
