# Install AlmaLinux10

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

In this case, you can jump to the [installation](#install-almalinux10-1) section.

If you have other distros installed, the output could look similar to the below:

```text
  NAME            STATE           VERSION
* AlmaLinux-8     Stopped         2
* AlmaLinux-9     Running         2
```

Make sure that the **STATE** column reads **Stopped** for all distros.
If any of them reads **Running**, you must stop if first by executing `wsl -t <distro-name>`, for example: `wsl -t AlmaLinux-9`.
Once you have stopped all distros, you can continue to the [installation](#install-almalinux10-1) section.

## Install AlmaLinux10

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
passwd: all authentication tokens updated successfully.
[<your-almalinux10-username>@<your-device-name> <your-windows-username>]$
```

## Setup AlmaLinux10

Install system packages:

```shell
sudo dnf install epel-release dnf-utils https://rpms.remirepo.net/enterprise/remi-release-10.rpm -y
```

You should see the below message, shown the first time you execute a command which requires elevated permissions (hence the `sudo` modifier at the beginning of the command).

```text
We trust you have received the usual lecture from the local System Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

For security reasons, the password you type will not be visible.

[sudo] password for dotkernel:
```

Input your AlmaLinux10 password and hit `Enter`.

Update/Upgrade system packages:

```shell
sudo dnf upgrade -y
```

Now, install the latest version of **Ansible Core** and run **ansible-galaxy** to install collections:

```shell
sudo dnf install ansible-core -y
```

```shell
ansible-galaxy collection install community.general community.mysql
```

Move inside your home directory (it is `/home/` followed by your AlmaLinux10 username, for example: `/home/dotkernel`):

```shell
cd ~
```

Clone the `alma-linux-10` branch of the `dotkernel/development` repository:

```shell
git clone --branch alma-linux-10 --single-branch https://github.com/dotkernel/development.git
```

Move inside the directory `development/wsl`:

```shell
cd development/wsl/
```

Duplicate `config.yml.dist` as `config.yml`:

```shell
cp config.yml.dist config.yml
```

Using your preferred text editor, open `config.yml` and fill in the empty fields.
Save and close the file.

Install components by running the below Ansible command:

```shell
ansible-playbook -i hosts install.yml --ask-become-pass
```

The installation process will ask for your AlmaLinux10 password, then iterate over each task in the playbook and output a short summary with the results.

Once finished, check if everything works by opening in your browser:

* [http://localhost/](http://localhost/): Apache's default home page
* [http://localhost/info.php](http://localhost/info.php): PHP info page
* [http://localhost/phpmyadmin/](http://localhost/phpmyadmin/): PhpMyAdmin (login with `root` + the root password you configured in `config.yml` under `mariadb` -> `root_password`)

The installation is complete, your AlmaLinux10 development environment is ready to use.

> Restart your `Windows Terminal` to find a new option in the tab selector, called **AlmaLinux-10**; clicking it will open a new tab connected to **AlmaLinux10**.

## Create virtualhosts

> By using the `*.localhost` pattern for any new virtualhost, you do not need to modify the `hosts` file in Windows, because these are routed by default.

Move inside the directory `development/wsl`:

```shell
cd ~/development/wsl/
```

If you don't already have a `config.yml` file, duplicate `config.yml.dist` as `config.yml`.

Using your preferred text editor, open `config.yml` and, under the `virtualhosts` key, enter the virtualhosts that you want to create, each on its own line.
Already existing virtualhosts will be skipped, their contents will not be lost, no need to comment or remove them.
Save and close the file.

Create the specified virtualhosts:

```shell
ansible-playbook -i hosts create-virtualhost.yml --ask-become-pass
```

This process will ask for your AlmaLinux10 password, iterate over the list of configured `virtualhosts` and output a short summary with the results.
Your virtualhost should be accessible and ready to use.

You will install your project under the `html` directory of your project, for example `/var/www/example.localhost/html`.

> The virtualhost's document root is set to the `public` directory of the above location, for example `/var/www/example.localhost/html/public`.

> If you want to have the DocumentRoot directly in `html` folder, you need to modify the file `/etc/httpd/sites-available/example.localhost`.

### Good to know

* To run your installed projects, you need to start AlmaLinux10 first.
* If you work with virtualhosts, your projects are created under `/var/www/`.
* You can still run PHP scripts under the default Apache project directory, located at `/var/www/html/`.
* If you encounter write permission issues, see [this guide](https://docs.dotkernel.org/development/v2/faq/#how-do-i-fix-common-permission-issues).
* We install PHP 8.4 by default—if you need a different version, see [this guide](https://docs.dotkernel.org/development/v2/faq/#how-do-i-switch-to-a-different-version-of-php).
* We install Node.js 22 by default—if you need a different version, see [this guide](https://docs.dotkernel.org/development/v2/faq/#how-do-i-switch-to-a-different-version-of-nodejs).
