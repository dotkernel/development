# Install Ansible development environment

[< Back](../README.md)


## Prerequisites
If you plan to develop on Windows, make sure to install [WSL2](wsl/README.md) before proceeding to the next step.

**Note**:
Make sure that Apache/MySQL are not running on your Windows machine (via XAMPP or any other tool) or in a different distro, else the installation will fail.


## Setup
Using your non-root system user, log into your Linux machine.

Update Linux distro packages using the below command:

    sudo apt-get update && sudo apt-get upgrade -y

It will prompt you for your password - you will not see what you're typing, that's a security measure in Linux regarding passwords.

If you're not already there, move to your home directory by executing `cd ~`.

Now execute the following command:

    sudo apt-get install ansible -y

This will automatically install the latest version of Ansible on your machine.

Clone dotkernel/development into your home directory using the below command:

    git clone https://github.com/dotkernel/development

Move inside the directory `development/ansible/{distro}` - where `{distro}` stands for the Linux distribution (in lowercase) your machine runs on.

For example, if you're on Ubuntu, type in this command:
```shell
cd development/ansible/ubuntu
```
Inside this directory, you will find `config.yml` - a generic config file used by all scripts across this playbook.

Open this file and fill in the empty fields:
* `system_user` = your non-root system user
* `mysql_root_password` = the password you will use for MySQL login
* `git_configurations`.`user.name` = your git account name
* `git_configurations`.`user.email` = your git account email
* `virtualhosts` = list of virtual hosts to be created (used only by the `create-virtualhost.yml` script - this can be filled in later)


## Install
Install and configure all necessary packages by running the below Ansible command:
```shell
ansible-playbook -i hosts install.yml
```
The installation process will iterate over each task in the playbook and will output a short summary with the results.

Now if you open in your browser [http://localhost/](http://localhost/), you should see Apache's default home page.

This installation is complete, your development environment is ready to use.


## Create virtual hosts
Create virtual hosts for your projects using the below command:

    ansible-playbook -i hosts create-virtualhost.yml

This will iterate over the `virtualhosts` configured in `config.yml` and will output a short summary with the results.

At this step, your virtual host is not yet accessible because first, you need to route these requests to localhost.

Open **as administrator** any text editor (Notepad, Sublime Text etc.).

Open file `C:\Windows\System32\drivers\etc\hosts` (if you don't see it, select `All files` from the file type selector).

For each item you placed in `virtualhosts`, add the below lines to the end of the file (make sure you replace `{virtualhost}` with your virtual host), then save the file.

    127.0.0.1	{virtualhost}
    ::1	{virtualhost}
Example:

    127.0.0.1	example.local
    ::1	example.local
    127.0.0.1	example2.local
    ::1	example2.local
Now, your virtual host should be accessible and ready to use.
