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

after you replace `{distro}` with the name of selected distro - for example:

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


## List installed distros
Open Windows Terminal and execute `wsl -l -v` or `wsl --list --verbose`.

Depending on the installed distros, the output of this command will look similar to the following:

| NAME         | STATE   | VERSION |
|--------------|---------|---------|
| Debian       | running | 2       |
| Ubuntu       | stopped | 2       |
| Ubuntu-16.04 | stopped | 2       |
| Ubuntu-18.04 | stopped | 2       |
| Ubuntu-18.04 | stopped | 2       |

Although the column names are self-explanatory, here's a few details on the columns:
* `NAME`: distro name - use this string when you need to replace `{distro}` with a value
* `STATE`: indicates distro status (`running` or `stopped`)
* `VERSION`: indicates if a distro runs on WSL1 or WSL2


## Access your distros
There are multiple ways for accessing a Linux distro:
* from Windows Terminal: use the arrow found in the tab bar (if you just installed a new distro, you need to restart the terminal for it to appear in the list)
* from Windows Terminal: type `wsl -d {distro}`, then hit `Enter`
* from Windows Explorer: type `\\wsl$\{distro}` in the address bar, then hit `Enter` (once there, you can also pin the directory to Quick access)


## Change hostname displayed in terminal (recommended)
When you have more than one distro installed, identifying which distro your terminal is connected to can become difficult.

In the below example we will modify the displayed hostname, by changing it to `ubuntu2004php74` - you can use any name you see fit.

[Access your distro](#access-your-distros) and go to your home directory. Then, locate and open `.bashrc` using your preferred text editor.

Around line 60, locate the following string:

> PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@**\h**\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

and replace it with:

> PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@**ubuntu2004php74**\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

Also, around line 62, locate the following string:

> PS1='${debian_chroot:+($debian_chroot)}\u@**\h**:\w\$ '

and replace it with:

> PS1='${debian_chroot:+($debian_chroot)}\u@**ubuntu2004php74**:\w\$ '

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

Get help anytime by typing `wsl -h` or `wsl --help`.

**Note**: You can also use this method to create forks of the same distro, each of them running different version of PHP.


## WSL and PhpStorm
Check out [this](https://www.jetbrains.com/help/phpstorm/how-to-use-wsl-development-environment-in-product.html) article on working with PhpStorm.


## WSL distro disk drive default locations
Below you will find the disk drive file locations for the most common distros:
* `Debian (latest)`: %USERPROFILE%\AppData\Local\Packages\TheDebianProject.DebianGNULinux_76v4gfsz19hv4\LocalState
* `Ubuntu (latest)`: %USERPROFILE%\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState
* `Ubuntu 16.04`: %USERPROFILE%\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu16.04onWindows_79rhkp1fndgsc\LocalState
* `Ubuntu 18.04`: %USERPROFILE%\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu18.04onWindows_79rhkp1fndgsc\LocalState
* `Ubuntu 20.04`: %USERPROFILE%\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu20.04onWindows_79rhkp1fndgsc\LocalState
