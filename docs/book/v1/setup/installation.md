# Installation

## Install AlmaLinux 9

Open Microsoft Store, in the search box type in: `AlmaLinux` and hit `Enter`.

From the results, select **AlmaLinux 9** - this will take you to **AlmaLinux 9**'s app page.

On this page, locate and click the `Install` button - this will download **AlmaLinux 9** WSL image on your system.

Once the download has finished, the `Install` button is replaced by an `Open` button - clicking it will open
`Windows Terminal`.

Here you will be asked to fill in your username (for example `dotkernel`):

```text
Installing, this may take a few minutes...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username:
```

Next, you are prompted to enter a password to use with your username (you will not see what you are typing, that's a
security measure in Linux regarding passwords):

```text
Enter new UNIX username: dotkernel.
Changing password for user dotkernel.
New password:
```

Depending on the strength of your password, you might see one of the following messages (if you want to choose a
different password, hit `Enter` and you are taken back to previous step - else, continue with retyping your password).

```text
BAD PASSWORD: The password fails the dictionary check - it is based on a dictionary word
BAD PASSWORD: The password is a palindrome
```

Next, you are asked to retype your password:

```text
Retype new password:
```

Finally, you should see the following message:

```text
passwd: all authentication tokens updated successfully.
Installation successful!
[dotkernel@hostname:~]$
```

## Setup AlmaLinux 9

Install requirements:

```shell
sudo dnf install epel-release dnf-utils http://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
```

Update/Upgrade system packages:

```shell
sudo dnf upgrade -y
```

Now, install the latest version of **Ansible**:

```shell
sudo dnf install ansible -y
```

Clone `dotkernel/development` into your home directory:

```shell
git clone https://github.com/dotkernel/development.git
```

Move inside the directory `development/wsl`:

```shell
cd ~/development/wsl/
```

Using your preferred text editor, open `config.yml` where you must fill in the empty fields.

Save and close the file.

Install requirements and initialize systemd by running the below Ansible command:

```shell
ansible-playbook -i hosts install.yml --ask-become-pass
```

The installation process will ask for your password (set during the installation process) and will iterate over each
task in the playbook and will output a short summary with the results.

At this step, **AlmaLinux 9** needs to be restarted - quit it by pressing `Control` + `d`.

Open `Windows Terminal`.

Stop **AlmaLinux 9**:

```shell
wsl -t AlmaLinux9
```

Start **AlmaLinux 9**:

```shell
wsl -d AlmaLinux9
```

Move inside the directory `development/wsl`:

```shell
cd ~/development/wsl/
```

Continue installation by running the below Ansible command:

```shell
ansible-playbook -i hosts install.yml --ask-become-pass
```

The installation process will ask for your password (set during the installation process) and will iterate over each
task in the playbook and will output a short summary with the results.

Now check if everything works by opening in your browser:

- [http://localhost/](http://localhost/) - Apache's default home page
- [http://localhost/info.php](http://localhost/info.php) - PHP info page
- [http://localhost/phpmyadmin/](http://localhost/phpmyadmin/) - PhpMyAdmin (login with `root` + the root password you
configured in `config.yml` under `mariadb` -> `root_password`)

The installation is complete, your **AlmaLinux 9** development environment is ready to use.

## Running AlmaLinux 9

Open `Windows Terminal`.

Start **AlmaLinux 9**:

```shell
wsl -d AlmaLinux9
```

### Note

> In order to run your applications using WSL2, you always need to be connected to your AlmaLinux9 distribution.
> For this, all you need to do is to keep open an instance of Windows Terminal that is connected to it.
