# Install Ansible development environment

[< DotKernel: Install development environment](../README.md)


## Prerequisites
If you plan to develop on Windows, make sure to install [WSL2](/ansible/wsl/README.md) before proceeding to the next step.


## Terminology
* `username` - your non-root username created at the distro installation.
* `distro` - the Linux distribution you are working on.


**Note**:
Make sure that Apache/MySQL are not running on your Windows machine (via XAMPP or any other tool) or in a different `distro`, or else the installation will fail (because service port is already in use).


## Setup
Using Windows Terminal, [start your distro](wsl/README.md#start-distro).

Make sure you are logged in with your `username` you've created during the installation of the `distro` by [checking your identity](wsl/README.md#check-identity).

If you are logged in as _root_, [switch to your user](wsl/README.md#switch-from-root-to-your-username).

Now you are ready to start the setup process.

Update/Upgrade Linux packages using the below command:

    sudo apt-get update && sudo apt-get upgrade -y

This command will prompt you for your password - you will not see what you're typing, that's a security measure in Linux regarding passwords.

If you're not already there, [Move to your home directory](HELP.md#move-to-your-home-directory).

Now, install the latest version of Ansible by running the following command:

    sudo apt-get install ansible -y

Clone dotkernel/development into your home directory using the below command:

    git clone https://github.com/dotkernel/development

Move inside `development/ansible` by executing:

    cd development/ansible/

Here, using your preferred text editor, open `config.yml` and fill in **the empty fields**. Save and close the file.


## Install
Install and configure all necessary packages by running the below Ansible command:

    ansible-playbook -i hosts install.yml --ask-become-pass

Once again, you will be prompted to enter your password.

The installation process will iterate over each task in the playbook and will output a short summary with the results.

Next, you need to [restart your distro](wsl/README.md#restart-distro) and start MySQL and Apache services.

Now check if everything works by opening in your browser:
* [http://localhost/](http://localhost/) - Apache's default home page
* [http://localhost/info.php](http://localhost/info.php) - PHP info page
* [http://localhost/phpmyadmin/](http://localhost/phpmyadmin/) - PhpMyAdmin (login with `root` + the root password you configured in `config.yml` under `mysql` -> `root_password`)

This installation is complete, your development environment is ready to use.


## Create virtual hosts
Create virtual hosts for your projects using the below command:

    ansible-playbook -i hosts create-virtualhost.yml --ask-become-pass

Once again, you will be prompted to enter your password.

This will iterate over the list of `virtualhosts` configured in `config.yml` and will output a short summary with the results.

At this step, your virtual host is not yet accessible because first, you need to route these requests to your localhost.

**Run as administrator** any text editor and open file `C:\Windows\System32\drivers\etc\hosts` (if you don't see it, select `All files` from the file type selector).

For each item you placed in `virtualhosts`, add the below lines to the end of the file (make sure you replace `{virtualhost}` with your virtual host), then save and close the file.

    127.0.0.1	{virtualhost}
    ::1	{virtualhost}
Example:

    127.0.0.1	example.local
    ::1	example.local
    127.0.0.1	example2.local
    ::1	example2.local

Your virtual host should be accessible and ready to use.

**Note**:
* You can still run PHP scripts under the default Apache project directory, located at `/var/www/html/`


## Help

## Find your projects directory
If you can't find your projects directory, see [this guide](HELP.md#move-to-your-home-directory).

### Project encounters write permission issues
You can fix the most common issues by consulting [this guide](HELP.md#fix-common-permission-issues).

### Switching between PHP versions
In order to switch from one version of PHP to another one, follow [this guide](HELP.md#switch-from-php74-to-php-81).
