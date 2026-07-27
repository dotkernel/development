# AlmaLinux 10 Setup

## Summary

Update system packages, install Ansible and required repositories, clone this repository, configure `config.yml`, and run the `install.yml` Ansible playbook to provision the full development environment.

> The instructions below also work without using WSL.

> This is step 3 of 3: [System Requirements](system-requirements.md) → [Install AlmaLinux 10](installation.md) → **Setup Packages** (this page).

> All commands on this page are executed **inside the AlmaLinux 10 shell** (Linux), not in Windows Terminal.
> This is the same session left open at the end of the [installation](installation.md) step.
> If you closed it, see [Running on WSL 2](../running.md) to reconnect first.

Update/Upgrade system packages:

```shell
sudo dnf upgrade -y
```

> This step downloads packages over the internet and can take a few minutes depending on your connection speed and how many updates are pending.

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

Input your **AlmaLinux 10** password and hit `Enter`.

Install system packages:

```shell
sudo dnf install epel-release dnf-utils https://rpms.remirepo.net/enterprise/remi-release-$(rpm -E %almalinux).rpm -y
```

> `$(rpm -E %almalinux)` resolves to your AlmaLinux major version (`10`), so this installs the matching Remi repository release package.
> `dnf` verifies the package's GPG signature before installing it, same as any other package.

Now, install the latest version of **Ansible Core** and run **ansible-galaxy** to install collections:

```shell
sudo dnf install ansible-core -y
```

```shell
ansible-galaxy collection install community.general community.mysql
```

> Both of the above require internet access to download packages/collections from their respective repositories (dnf repos and Ansible Galaxy).
> If either command fails or times out, check your network connection first.

Move inside your home directory (it is `/home/` followed by your **AlmaLinux 10** username, for example: `/home/dotkernel`):

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

Using your preferred text editor, open `config.yml` and fill in the empty fields:

* `config.git.config."user.name"` / `"user.email"`: the identity Git will use for your commits inside AlmaLinux 10.
* `config.mariadb.root_password`: the password to set for MariaDB's `root` user (you'll use this to log into phpMyAdmin later).
* `config.system.username`: defaults to your current AlmaLinux 10 username automatically, usually no change needed.
* `config.virtualhosts`: a list of local domains to create virtualhosts for (see [Virtualhosts](../virtualhosts/overview.md)); the default entry `example.localhost` can be removed or left as-is.

Save and close the file.

Install components by running the below Ansible command:

```shell
ansible-playbook -i hosts install.yml --ask-become-pass
```

The installation process will ask for your **AlmaLinux 10** password, then iterate over each task in the playbook, and output a short summary with the results.

Once finished, check if everything works by opening in your browser:

> If you are not using WSL 2, test the below using your server's IP address instead of `localhost`.

* [http://localhost/](http://localhost/): Apache's default home page
* [http://localhost/info.php](http://localhost/info.php): PHP info page
* [http://localhost/phpmyadmin/](http://localhost/phpmyadmin/): PhpMyAdmin (login with `root` + the root password you configured in `config.yml` under `mariadb` -> `root_password`)

The installation is complete, your **AlmaLinux 10** development environment is ready to use.

> If you are using WSL 2, restart your `Windows Terminal` to find a new option in the tab selector, called **AlmaLinux-10**.
> Clicking it will open a new tab connected to **AlmaLinux 10**.

## Next step

Your environment is now provisioned. Continue to [Running on WSL 2](../running.md) to learn how to (re)connect to your **AlmaLinux 10** distro, or jump straight to [Virtualhosts](../virtualhosts/overview.md) to host your first project.

## FAQ

### What if `config.yml` doesn't exist yet?

Duplicate the provided template by running:

```shell
cp config.yml.dist config.yml
```

Then fill in the empty fields before running the playbook.

### What if the Ansible playbook fails partway through?

Review the summary output for the failed task, fix the underlying issue (for example, a misconfigured field in `config.yml`), and re-run:

```shell
ansible-playbook -i hosts install.yml --ask-become-pass
```

It is safe to re-run.

### What if `ansible-galaxy collection install` fails or times out?

This command downloads collections from Ansible Galaxy over the internet, so it fails if your network connection is down, unstable, or blocked by a proxy/firewall.
Check your connection and re-run the command.
It's safe to run again even if some collections already installed successfully.

### What if `http://localhost/` doesn't load after installation?

If you are not using WSL 2, make sure you are using your server's IP address instead of `localhost`.
Otherwise, confirm the playbook completed without errors and that your **AlmaLinux 10** distro is still running.

### How do I log into phpMyAdmin?

Use the username `root` and the password you configured under `mariadb` -> `root_password` in `config.yml`.

### Can I run this setup without WSL?

Yes, the instructions work the same way on a non-WSL AlmaLinux system.
You can then skip the WSL-specific notes.
