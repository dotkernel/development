# Install WSL2 development environment on AlmaLinux9

[< Dotkernel: Install development environment](../../README.md)

## Download AlmaLinux9 image

Download **AlmaLinux9** image by following one of the below methods.

### Method 1: Download AlmaLinux9 WSL2 image using Windows Terminal

Open `Windows Terminal` and execute the following command:

```shell
wsl --install -d AlmaLinux-9
```

You should see the download progress, and once finished, the output should look like this:

```text
Downloading: AlmaLinux 9
Installing: AlmaLinux 9
AlmaLinux 9 has been installed.
Launching AlmaLinux 9...
```

Also, you should find a new tab in `Windows Terminal` that is already connected to **AlmaLinux9**.

### Method 2: Download AlmaLinux9 WSL2 image from Microsoft Store

Open Microsoft Store, type `AlmaLinux` in the search box and hit `Enter`.

From the results, select `AlmaLinux OS 9` - this will take you to AlmaLinux9's app page.

On this page, locate and click the `Install` button - this will download AlmaLinux9 WSL2 image on your machine.

Once the download has finished, the `Install` button is replaced by an `Open` button - clicking it will open `Windows Terminal`.

## Install AlmaLinux9

You will be asked to fill in your username (for example `dotkernel`):

```text
Installing, this may take a few minutes...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username:
```

Next, you are prompted to enter a password to use with your username (you will not see what you are typing, that's a security measure in Linux regarding passwords):

```shell
Enter new UNIX username: dotkernel.
Changing password for user dotkernel.
New password:
```

Depending on the strength of your password, you might see one of the following messages (if you want to choose a different password, hit `Enter` and you are taken back to previous step - else, continue with retyping your password):

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

## Setup AlmaLinux9

Install requirements:

```shell
sudo dnf install epel-release dnf-utils https://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
```

Update/Upgrade system packages:

```shell
sudo dnf upgrade -y
```

Now, install the latest version of Ansible:

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

Duplicate `config.yml.dist` as `config.yml`.

Using your preferred text editor, open `config.yml` and fill in the empty fields.

Save and close the file.

Install requirements and initialize systemd by running the below Ansible command:

```shell
ansible-playbook -i hosts install.yml --ask-become-pass
```

The installation process will iterate over each task in the playbook and will output a short summary with the results.

AlmaLinux9 needs to be restarted, so quit it by pressing `Control` + `d`.

Open Windows Terminal.

Stop AlmaLinux9:

```shell
wsl -t AlmaLinux9
```

Start AlmaLinux9:

```shell
wsl -d AlmaLinux9
```

Move inside the directory `development/wsl`:

```shell
cd ~/development/wsl/
```

Continue the installation by running the below Ansible command:

```shell
ansible-playbook -i hosts install.yml --ask-become-pass
```

The installation process will iterate over each task in the playbook and will output a short summary with the results.

Now check if everything works by opening in your browser:

* [http://localhost/](http://localhost/): Apache's default home page
* [http://localhost/info.php](http://localhost/info.php): PHP info page
* [http://localhost/phpmyadmin/](http://localhost/phpmyadmin/): PhpMyAdmin (login with `root` + the root password you configured in `config.yml` under `mariadb` -> `root_password`)

The installation is complete, your development environment is ready to use.

## Create virtualhosts

Move inside the directory `development/wsl`:

```shell
cd ~/development/wsl/
```

Using your preferred text editor, open `config.yml` and, under the `virtualhosts` key, enter the virtualhosts that you want to create, each on its own line. 

Already existing ones will be skipped, no need to comment or remove them.

> By using the `*.localhost` pattern for any new virtualhost, you do not need to modify the `hosts` file in Windows, because these are routed by default.

Save and close the file.

Create the specified virtualhosts:

```shell
ansible-playbook -i hosts create-virtualhost.yml --ask-become-pass
```

This will iterate over the list of configured `virtualhosts` and will output a short summary with the results.

Your virtualhost should be accessible and ready to use.

You will install your project under the `html` directory of your project, for example: `/var/www/example.localhost/html`.

> The virtualhost's document root is set to the `public` directory of the above location, for example `/var/www/example.localhost/html/public`.

> If you want to have the DocumentRoot directly in `html` folder, you need to modify the file `/etc/httpd/sites-available/example.localhost`.

### Good to know

* To run your installed projects, you need to start AlmaLinux9 first.
* If you work with virtualhosts, your projects are created under `/var/www/`.
* You can still run PHP scripts under the default Apache project directory, located at `/var/www/html/`.
* If you encounter write permission issues, see [this guide](FAQ.md#how-do-i-fix-common-permission-issues).
* We install PHP 8.3 by default. If you need a different version, see [this guide](FAQ.md#how-do-i-switch-to-a-different-version-of-php).
* We install Node.js 22 by default. If you need a different version, see [this guide](FAQ.md#how-do-i-switch-to-a-different-version-of-nodejs).
