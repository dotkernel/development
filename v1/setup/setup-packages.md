# AlmaLinux 9 Setup

> The instructions below also work without using WSL.

Update/Upgrade system packages:

```shell
sudo dnf upgrade -y
```

You should see the below message, shown the first time you execute a command which requires elevated permissions (hence the `sudo` modifier at the beginning of the command).

```text
We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for dotkernel:
```

Input your **AlmaLinux 9** password and hit `Enter`.

Install system packages:

```shell
sudo dnf install epel-release dnf-utils https://rpms.remirepo.net/enterprise/remi-release-$(rpm -E %almalinux).rpm -y
```

Now, install the latest version of **Ansible**:

```shell
sudo dnf install ansible -y
```

Move inside your home directory (it is `/home/` followed by your **AlmaLinux 9** username, for example: `/home/dotkernel`):

```shell
cd ~
```

Clone the `alma-linux-9` branch of the `dotkernel/development` repository:

```shell
git clone --branch alma-linux-9 --single-branch https://github.com/dotkernel/development.git
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

The installation process will ask for your **AlmaLinux 9** password, then iterate over each task in the playbook and output a short summary with the results.

Once finished, check if everything works by opening in your browser:

> If you are not using WSL 2, test the below using your server's IP address instead of `localhost`.

* [http://localhost/](http://localhost/): Apache's default home page
* [http://localhost/info.php](http://localhost/info.php): PHP info page
* [http://localhost/phpmyadmin/](http://localhost/phpmyadmin/): PhpMyAdmin (login with `root` + the root password you configured in `config.yml` under `mariadb` -> `root_password`)

The installation is complete, your **AlmaLinux 9** development environment is ready to use.

> If you are using WSL 2, restart your `Windows Terminal` to find a new option in the tab selector, called **AlmaLinux-9** - clicking it will open a new tab connected to **AlmaLinux 9**.
