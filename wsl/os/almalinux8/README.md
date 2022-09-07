# Install WSL development environment on AlmaLinux 8

[< DotKernel: Install development environment](../../README.md)


## Warning
Not fully functional yet, because there's a known issue running systemd as PID1 inside WSL2.

This causes crucial services (Apache, MySQL) not being possible to be started.

If you still want to give it a try, you can proceed with the installation.


## Download AlmaLinux 8 WSL image
Open Microsoft Store, in the search box type in: `AlmaLinux` and hit `Enter`.

From the results, select `AlmaLinux 8 WSL` - this will take you to AlmaLinux 8's app page.

On this page, locate and click the `Install` button - this will download AlmaLinux 8 WSL image on your machine.

Once the download has finished, the `Install` button is replaced by an `Open` button - clicking it will open Windows Terminal.

Here you will be asked to fill in your username (for example `dotkernel`):

    Installing, this may take a few minutes...
    Please create a default UNIX user account. The username does not need to match your Windows username.
    For more information visit: https://aka.ms/wslusers
    Enter new UNIX username:

Next, you are prompted to enter a password to use with your username (you will not see what you are typing, that's a security measure in Linux regarding passwords):

    Enter new UNIX username: dotkernel.
    Changing password for user dotkernel.
    New password:

Depending on the strength of your password, you might see the following message (if you want to choose a different password, hit `Enter` and you are taken back to previous step - else, continue)

    BAD PASSWORD: The password fails the dictionary check - it is based on a dictionary word

Next, you asked to retype your password:

    Retype new password:

Finally, you should see the following message:

    passwd: all authentication tokens updated successfully.
    Installation successful!
    [dotkernel@hostname:~]$


## Install AlmaLinux 8
Make sure you are in your home directory:

    cd ~

Update/Upgrade system packages:

    sudo dnf upgrade -y

Install Extra Packages for Enterprise Linux (EPEL):

    sudo dnf install epel-release -y

Now, install the latest version of Ansible:

    sudo dnf install ansible -y

Clone dotkernel/development into your home directory:

    git clone https://github.com/dotkernel/development.git

Move inside the directory `development/wsl`:

    cd development/wsl/

Using your preferred text editor, open `config.yml` where you must fill in the empty fields.

Save and close the file.

Install and configure all necessary services by running the below Ansible command:

    ansible-playbook -i hosts install.yml --ask-become-pass

The installation process will iterate over each task in the playbook and will output a short summary with the results.

At this point, AlmaLinux 8 needs to be restarted, so quit it by pressing `Control` + `d`.

Open Windows Terminal.

Stop AlmaLinux 8:

    wsl -t AlmaLinux-8

Start AlmaLinux 8:

    wsl -d AlmaLinux-8

Move to your home directory:

    cd ~

Start MySQL:

    sudo service mysql start

Start Apache:

    sudo service apache2 start

Now check if everything works by opening in your browser:
* [http://localhost/](http://localhost/) - Apache's default home page
* [http://localhost/info.php](http://localhost/info.php) - PHP info page
* [http://localhost/phpmyadmin/](http://localhost/phpmyadmin/) - PhpMyAdmin (login with `root` + the root password you configured in `config.yml` under `mysql` -> `root_password`)

The installation is complete, your development environment is ready to use.


## Create virtual hosts
Using your preferred text editor, open `config.yml` and add the hosts that you want to create under the `virtualhosts` list. Save and close the file.

Create the specified virtual hosts:

    ansible-playbook -i hosts create-virtualhost.yml --ask-become-pass

This will iterate over the list of `virtualhosts` configured in `config.yml` and will output a short summary with the results.

At this step, your virtual host is not yet accessible because first, you need to route the requests to your localhost.

**Run as administrator** any text editor and open file `C:\Windows\System32\drivers\etc\hosts` (if you don't see it, select `All files` from the file type selector).

For each item you placed in the `virtualhosts` list, add the below lines to the end of the file (make sure you replace `{virtualhost}` with your virtual host), then save and close the file.

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
* If you encounter write permission issues, see [this guide](../../HELP.md#fix-common-permission-issues)
