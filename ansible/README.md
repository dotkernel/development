# Install Ansible development environment

[< Back](../README.md)

## Install:
If you plan to develop on Windows, make sure to install [WSL2](wsl.md) before proceeding to the next step.

### Step 1
Once logged into your Linux machine, install Ansible using  the following command:
```bash
apt-get install ansible -y
```

### Step 2
Git clone dotkernel/development using:
```shell
git clone https://github.com/dotkernel/development
```
Enter directory `development/ansible/{distro}` in order to install requirements for that specific distribution.

Open `config.yml` and fill in the variables left empty.

Run the Ansible command to install the environment, using:
```shell
ansible-playbook -i hosts install.yml
```

### Step 3
Create virtualhosts for your projects using the command:
```shell
ansible-playbook -i hosts create-virtualhost.yml
```
