# Install WSL development environment on Ubuntu 22

[< DotKernel: Install development environment](../../../README.md)


## Download Ubuntu 22 WSL image
Open Microsoft Store, in the search box type in: `Ubuntu` and hit `Enter`.

From the results, select `Ubuntu 22.04.1 LTS` - this will take you to Ubuntu 22's app page.

On this page, locate and click the `Install` button - this will download Ubuntu 22 WSL image on your machine.

Once the download has finished, the `Install` button is replaced by an `Open` button - clicking it will open Windows Terminal.

An installer window will open - once it finished processing, you will see a popup:

    We are almost done
    Just a few steps to be completed in the main installer window.
    Can we quit this one and go there?

Click `Ok` to close the window or wait a few seconds, and it will close automatically.

Back in the main installer, follow the on-screen instructions and go through each page.


## Install Ubuntu 22
Make sure you are in your home directory:

    cd ~

Update/Upgrade system packages:

    sudo apt-get update && sudo apt-get upgrade -y

Now, install the latest version of Ansible:

    sudo apt-get install ansible -y

Clone dotkernel/development into your home directory:

    git clone https://github.com/dotkernel/development.git

Move inside the directory `development/wsl`:

    cd development/wsl/

Using your preferred text editor, open `config.yml` where you must fill in the empty fields.

Save and close the file.

Install and configure all necessary services by running the below Ansible command:

    ansible-playbook -i hosts install.yml --ask-become-pass

The installation process will iterate over each task in the playbook and will output a short summary with the results.

At this point, Ubuntu 22 needs to be restarted, so quit it by pressing `Control` + `d`.

Open Windows Terminal.

Stop Ubuntu 22:

    wsl -t Ubuntu-22.04

Start Ubuntu 22:

    wsl -d Ubuntu-22.04

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
