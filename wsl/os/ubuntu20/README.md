# Install WSL development environment on Ubuntu 20

[< DotKernel: Install development environment](../../README.md)


## Download Ubuntu 20 WSL image
Download Ubuntu 20 image by following one of the below methods.


### Method 1: Download Ubuntu 20 WSL image using Windows Terminal
Open Windows Terminal and execute the following command:

    wsl --install -d Ubuntu-20.04

You should see the download progress and once finished, the output should look like this:

    Downloading: Ubuntu 20.04 LTS
    Installing: Ubuntu 20.04 LTS
    Ubuntu 20.04 LTS has been installed.
    Launching Ubuntu 20.04 LTS...

Also, you should find a new tab in Windows Terminal that is already connected to Ubuntu 20.


### Method 2: Download Ubuntu 20 WSL image from Microsoft Store
Open Microsoft Store, in the search box type in: `Ubuntu` and hit `Enter`.

From the results, select `Ubuntu 20.04.4 LTS` - this will take you to Ubuntu 20's app page.

On this page, locate and click the `Install` button - this will download Ubuntu 20 WSL image on your machine.

Once the download has finished, the `Install` button is replaced by an `Open` button - clicking it will open Windows Terminal.


## Install Ubuntu 20
You will be asked to fill in your username (for example `dotkernel`):

    Installing, this may take a few minutes...
    Please create a default UNIX user account. The username does not need to match your Windows username.
    For more information visit: https://aka.ms/wslusers
    Enter new UNIX username:

Next, you are prompted to enter a password to use with your username (you will not see what you are typing, that's a security measure in Linux regarding passwords):

    Enter new UNIX username: dotkernel.
    Changing password for user dotkernel.
    New password:

Next, you are asked to retype your password:

    Retype new password:

Finally, you should see the following message:

    passwd: password updated successfully
    Installation successful!
    ...
    dotkernel@hostname:~$


## Install services on Ubuntu 20
Update/Upgrade system packages:

    sudo apt-get update && sudo apt-get upgrade -y

Now, install the latest version of Ansible:

    sudo apt-get install ansible -y

Clone dotkernel/development into your home directory:

    git clone https://github.com/dotkernel/development.git

Move inside the directory `development/wsl`:

    cd ~/development/wsl/

Using your preferred text editor, open `config.yml` where you must fill in the empty fields.

Save and close the file.

Install and configure all necessary services by running the below Ansible command:

    ansible-playbook -i hosts install.yml --ask-become-pass

The installation process will iterate over each task in the playbook and will output a short summary with the results.

At this point, Ubuntu 20 needs to be restarted, so quit it by pressing `Control` + `d`.

Open Windows Terminal.

Stop Ubuntu 20:

    wsl -t Ubuntu-20.04

Start Ubuntu 20:

    wsl -d Ubuntu-20.04

Now check if everything works by opening in your browser:
* [http://localhost/](http://localhost/) - Apache's default home page
* [http://localhost/info.php](http://localhost/info.php) - PHP info page
* [http://localhost/phpmyadmin/](http://localhost/phpmyadmin/) - PhpMyAdmin (login with `root` + the root password you configured in `config.yml` under `mariadb` -> `root_password`)

The installation is complete, your development environment is ready to use.


## Create virtual hosts
Move inside the directory `development/wsl`:

    cd ~/development/wsl/

Using your preferred text editor, open `config.yml` and, under the `virtualhosts` key, enter the virtualhosts that you want to create, each on its own line.

Already existing ones will be skipped, no need to comment or remove them.

Save and close the file.

Create the specified virtualhosts:

    ansible-playbook -i hosts create-virtualhost.yml --ask-become-pass

This will iterate over the list of configured `virtualhosts` and will output a short summary with the results.

Your virtualhost should be accessible and ready to use.

You will install your projects under the `/home/your-username/projects/` directory.
The virtualhost's document root is set to the `public` directory of the above location, for example `/home/your-username/projects/example.local/public`.

**Note**:
* In order to run your installed projects, you need to start Ubuntu 20 first.
* If you work with virtualhosts, your projects are created under `/home/your-username/projects/`.
* You can still run PHP scripts under the default Apache project directory, located at `/var/www/html/`.
* If you encounter write permission issues, see [this guide](FAQ.md#how-do-i-fix-common-permission-issues).
* Default version of PHP is set to 8.3. If you need a different version, see [this guide](FAQ.md#how-do-i-switch-between-php-versions).
* We install NodeJS 22 by default. If you need a different version, see [this guide](FAQ.md#how-do-i-switch-to-a-different-version-of-nodejs).
