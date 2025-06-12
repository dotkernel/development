# Setup AlmaLinux10

Install system packages:

```shell
sudo dnf install epel-release dnf-utils https://rpms.remirepo.net/enterprise/remi-release-10.rpm -y
```

You should see the below message, shown the first time you execute a command which requires elevated permissions (hence the `sudo` modifier at the beginning of the command).

```text
We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

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

Now, install the latest version of **Ansible Core** and run **ansible-galaxy** in order to install collections:

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

## Running AlmaLinux10

Open `Windows Terminal`.

Start AlmaLinux10 by executing:

```shell
wsl -d AlmaLinux10
```

OR

Locate the app selector dropdown in `Windows Terminal`'s title bar and click `AlmaLinux-10`.
This will open a new tab connected to AlmaLinux10.

### Note

> To run your applications using WSL2, you always need to be connected to your AlmaLinux10 distribution.
> For this, all you need to do is to keep open an instance of Windows Terminal that is connected to it.
