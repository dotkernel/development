# Install WSL2

[< Back](README.md)

## Prerequisites
Before proceeding to the installation, we need to make sure your machine is ready for running WSL2.

Open the `Run` prompt by pressing `Win`+`r` and type in the dialog `OptionalFeatures`, then press `Enter`.
This will open a window where you can turn Windows features on/off.
Make sure the next features are activated (checked):
* `Hyper-V` (including its sub-features)
* `Windows Subsystem for Linux`

Click `Ok` and restart your machine.


## Install
Open Windows PowerShell and type in the below command:
```shell
wsl --install
```
This command will:
* enable the required optional components
* download the latest Linux kernel
* set WSL 2 as your default
* install a Linux distribution for you (Ubuntu by default)

Once completed, a new terminal will open, prompting you to:
* `Enter new UNIX username:` - enter the username you will use to log in to this machine
* `Enter new UNIX password:` - enter the password you will use in combination with the selected username (you will not see what you're typing, that's a security measure in Linux regarding passwords)
* `Retype new UNIX password:` - repeat your password

At the end, you should see the below output:

    passwd: password updated successfully
    Installation successful!

More on installing WSL and other distributions can be found [here](https://docs.microsoft.com/en-us/windows/wsl/install)


## Change hostname displayed in terminal:
When you have more than one machines installed, knowing which machine your terminal is connected to can become difficult.
The following solution should fix this.

Open Windows Explorer and paste the following in your address bar:

    \\wsl$\{distro}\home\{username}
after replacing:
* `{distro}` with the name of the Linux distribution you are logged in
* `{username}` with you system user (the one you used at installation)

Example:

    \\wsl$\Ubuntu\home\username

Locate the file `.bashrc` and open it using your preferred text editor.

Around line 60, locate the following:

    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
and replace it with

    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@__HOSTNAME__\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
where `__HOSTNAME__` must be replaced with a more relevant name (example: `ubuntu20php74`).

Also, around line 62, locate the following:

    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
and replace it with:

    PS1='${debian_chroot:+($debian_chroot)}\u@__HOSTNAME__:\w\$ '
where again, `__HOSTNAME__` must be replaced with the same relevant name used above.

Save the file and close it.

Next time you log in to your machine, the terminal will reflect the machine's name.
