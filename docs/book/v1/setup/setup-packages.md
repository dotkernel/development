# Setup AlmaLinux9

Install system packages:

```shell
sudo dnf install epel-release dnf-utils https://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
```

You should see the below message, shown the first time you execute a command which requires elevated permissions (hence the `sudo` modifier at the beginning of the command).

```text
We trust you have received the usual lecture from the local System Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for dotkernel:
```

Input your AlmaLinux9 password and hit `Enter`.

Update/Upgrade system packages:

```shell
sudo dnf upgrade -y
```

Now, install the latest version of **Ansible**:

```shell
sudo dnf install ansible -y
```

Move inside your home directory (it is `/home/` followed by your AlmaLinux9 username, for example: `/home/dotkernel`):

```shell
cd ~
```

Clone the `almalinux9` branch of the `dotkernel/development` repository:

```shell
git clone --branch almalinux9 --single-branch https://github.com/dotkernel/development.git
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

The installation process will ask for your AlmaLinux9 password, then iterate over each task in the playbook and output a short summary with the results.

Once finished, check if everything works by opening in your browser:

* [http://localhost/](http://localhost/): Apache's default home page
* [http://localhost/info.php](http://localhost/info.php): PHP info page
* [http://localhost/phpmyadmin/](http://localhost/phpmyadmin/): PhpMyAdmin (login with `root` + the root password you configured in `config.yml` under `mariadb` -> `root_password`)

The installation is complete, your AlmaLinux9 development environment is ready to use.

> Restart your `Windows Terminal` to find a new option in the tab selector, called **AlmaLinux-9** - clicking it will open a new tab connected to **AlmaLinux9**.

## Running AlmaLinux9

Open `Windows Terminal`.

Start AlmaLinux9 by executing:

```shell
wsl -d AlmaLinux9
```

OR

Locate the app selector dropdown in `Windows Terminal`'s title bar and click `AlmaLinux-9`.
This will open a new tab connected to AlmaLinux9.

### Note

> To run your applications using WSL2, you always need to be connected to your AlmaLinux9 distribution.
> For this, all you need to do is to keep open an instance of Windows Terminal that is connected to it.
