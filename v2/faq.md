# Frequently asked questions

Quick answers to common questions about the security posture of this environment, troubleshooting systemd/port issues, switching PHP/Node.js versions, fixing permissions, finding logs, and maintaining your development environment.

* [Is this environment safe to expose beyond localhost?](#is-this-environment-safe-to-expose-beyond-localhost)
* [Why do playbook tasks fail with a systemd error?](#why-do-playbook-tasks-fail-with-a-systemd-error)
* [What if port 80 is already in use by something else?](#what-if-port-80-is-already-in-use-by-something-else)
* [How do I switch to a different version of PHP?](#how-do-i-switch-to-a-different-version-of-php)
* [How do I switch to a different version of Node.js?](#how-do-i-switch-to-a-different-version-of-nodejs)
* [How do I fix common permission issues?](#how-do-i-fix-common-permission-issues)
* [Where are the error log files?](#where-are-the-error-log-files)
* [How do I update Composer?](#how-do-i-update-composer)
* [How do I update phpMyAdmin?](#how-do-i-update-phpmyadmin)
* [How do I upgrade MariaDB?](#how-do-i-upgrade-mariadb)
* [Why are my git credentials stored in plaintext, and can I use something else?](#why-are-my-git-credentials-stored-in-plaintext-and-can-i-use-something-else)
* [How do I delete a virtualhost?](#how-do-i-delete-a-virtualhost)
* [How do I remove the provisioned stack without deleting the whole distro?](#how-do-i-remove-the-provisioned-stack-without-deleting-the-whole-distro)
* [How do I create command aliases?](#how-do-i-create-command-aliases)

## Is this environment safe to expose beyond localhost?

No — this stack is a **local development environment only**, not intended for production or network-exposed use. Security is relaxed or effectively nonexistent by default: the MariaDB root password lives in plaintext in `config.yml`, phpMyAdmin is served at a predictable `/phpmyadmin` path with root login, and nothing is firewalled off.

`firewalld` is installed by the playbook but never enabled or given any rule — it's there so you have the option, not because anything is locked down out of the box. If you ever need to open a port (for example, to reach Apache from another device on your network), enable it and add a rule explicitly:

```shell
sudo systemctl enable --now firewalld
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --reload
```

Only do this if you understand the rest of the environment is otherwise unsecured by default — don't expose it on an untrusted network.

## Why do playbook tasks fail with a systemd error?

If `install.yml` fails on a task like `systemctl enable --now httpd` with an error such as `System has not been booted with systemd as init system`, systemd isn't active yet inside your **AlmaLinux 10** WSL distro.

Check with:

```shell
systemctl is-system-running
```

If that errors instead of printing a status, add the following to `/etc/wsl.conf` (you'll need `sudo` to edit it):

```text
[boot]
systemd=true
```

Then, from **Windows Terminal** (not inside AlmaLinux 10), restart the distro for the change to take effect:

```shell
wsl --shutdown
```

Reopen **AlmaLinux 10**, re-run the check above to confirm, then re-run `install.yml` — it's safe to re-run.

## What if port 80 is already in use by something else?

Apache needs port 80. If Docker Desktop, IIS, Skype, or another service on your Windows host already holds it, Apache will fail to bind (and `install.yml` will fail at the Apache step).

To find what's using port 80 on Windows, run in `Windows Terminal`:

```shell
netstat -ano | findstr :80
```

Then either stop the conflicting service, or change Apache's `Listen` port inside AlmaLinux 10 and access your virtualhosts on that port instead:

```shell
sudo nano /etc/httpd/conf/httpd.conf
```

Update the `Listen` directive, then restart httpd:

```shell
sudo systemctl restart httpd
```

## How do I switch to a different version of PHP?

Execute the following command:

```shell
sudo dnf module switch-to php:remi-{major}.{minor} -y
```

where `{major}.{minor}` is one of the supported PHP versions: `8.5`, `8.4`, `8.3`, `8.2` and `8.1`.

Additionally, our setup includes predefined aliases for executing the above command.
The aliases are the following:

* `php81`: switch to PHP 8.1
* `php82`: switch to PHP 8.2
* `php83`: switch to PHP 8.3
* `php84`: switch to PHP 8.4
* `php85`: switch to PHP 8.5

After switching to a different PHP version, test with the following command:

```shell
php -v
```

Depending on the selected PHP version, the output should look similar to the below:

```terminaloutput
PHP 8.4.8 (cli) (built: Jun  3 2025 16:29:26) (NTS gcc x86_64)
Copyright (c) The PHP Group
Built by Remi's RPM repository <https://rpms.remirepo.net/> #StandWithUkraine
Zend Engine v4.4.8, Copyright (c) Zend Technologies
```

## How do I switch to a different version of Node.js?

Execute the following commands:

```shell
sudo dnf remove nodejs -y
curl -fsSL https://rpm.nodesource.com/setup_{major}.x | sudo bash -
sudo dnf install nodejs -y
```

where `{major}` is the Node.js version you want to switch to.

Additionally, our setup includes predefined aliases for the above commands.
The aliases are the following:

* `node24`: switch to Node.js 24
* `node22`: switch to Node.js 22
* `node20`: switch to Node.js 20
* `node18`: switch to Node.js 18

After switching to a different Node.js version, test with the following command:

```shell
node -v
```

Depending on the selected Node.js version, the output should look similar to the below:

```terminaloutput
v24.13.0
```

Check npm version:

```shell
npm -v
```

Depending on the current npm version, the output should look similar to the below:

```terminaloutput
11.9.0
```

## How do I fix common permission issues?

If running your project, you encounter permission issues, follow the below steps.

> `chmod -R 777` grants read/write/execute access to everyone, which is only appropriate for a local development environment like this one.
> Avoid carrying this habit into staging or production, where permissions should be scoped more narrowly (for example, to the web server's user/group).

### Error

> PHP Fatal error: Uncaught InvalidArgumentException: The directory "`<path-to-project>`/data" is not writable...

> PHP Fatal error: Uncaught InvalidArgumentException: The directory "`<path-to-project>`/data/cache" is not writable...

> PHP Fatal error: Uncaught InvalidArgumentException: The directory "`<path-to-project>`/data/cache/doctrine" is not writable...

### Solution

```shell
chmod -R 777 data
```

### Error

> PHP Fatal error: Uncaught InvalidArgumentException: The directory "`<path-to-project>`/public/uploads" is not writable...

### Solution

```shell
chmod -R 777 public/uploads
```

### Error

> PHP Fatal error: Uncaught ErrorException: fopen(`<path-to-project>`/log/error-log-yyyy-mm-dd.log): Failed to open stream: Permission denied...

### Solution

```shell
chmod -R 777 log
```

## Where are the error log files?

From time to time, you are encountering various errors which are not displayed. Or you can get errors 500 in a browser.

To find the error messages, you need to read the error log files.

### Apache log files

```text
/var/log/httpd/error_log
```

### PHP-FPM log files

```text
/var/log/php-fpm/error.log
/var/log/php-fpm/www-error.log
```

## How do I update Composer?

Before updating, check your current Composer version by executing:

```shell
composer --version
```

The output should be similar to:

```text
Composer version 2.8.5 2025-01-21 15:23:40
PHP version 8.5.0 (/usr/bin/php)
Run the "diagnose" command to get more detailed diagnostics output.
```

Update Composer using its own `self-update` command:

```shell
sudo /usr/local/bin/composer self-update
```

The output should be similar to:

```text
Upgrading to version 2.9.0 (stable channel).

Use composer self-update --rollback to return to version 2.8.5
```

After updating, check again your Composer version by executing:

```shell
composer --version
```

The output should be similar to (note that only the Composer version changes — `self-update` never touches your PHP version):

```text
Composer version 2.9.0 2025-11-13 10:37:16
PHP version 8.5.0 (/usr/bin/php)
Run the "diagnose" command to get more detailed diagnostics output.
```

## How do I update phpMyAdmin?

Being installed as a system package, it can be updated using the command which updates the rest of the system packages:

```shell
sudo dnf upgrade -y
```

## How do I upgrade MariaDB?

This repo currently pins MariaDB to the version set in `wsl/roles/mariadb/templates/MariaDB.repo.j2` (`12.3` at the time of writing — check that file for the current value, since it has changed more than once during development). If you want to switch to a different version, use the below steps:

* Open the MariaDB.repo file in any text editor.

```shell
sudo nano /etc/yum.repos.d/MariaDB.repo
```

* Modify the **baseurl** variable to match the desired version, replacing the version number currently in the URL with the one you want.
* Clean dnf cache

```shell
sudo dnf clean all
```

* Stop MariaDB

```shell
sudo systemctl stop mariadb
```

* Upgrade MariaDB

```shell
sudo dnf update -y
```

* Start MariaDB

```shell
sudo systemctl start mariadb
```

* Upgrade databases

```shell
sudo mariadb-upgrade -uroot -p
```

* Restart MariaDB

```shell
sudo systemctl restart mariadb
```

## Why are my git credentials stored in plaintext, and can I use something else?

The `git` role sets `credential.helper=store`, which saves your credentials to a plaintext file after your first successful git operation. That's convenient for a local dev box, but the file isn't encrypted.

Alternatives:

* Point WSL's git credential helper at the Windows Git Credential Manager executable, so credentials are shared with (and encrypted by) Windows instead of stored in plaintext inside WSL:

```shell
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager.exe"
```

* Or use `gh` (GitHub CLI) / `glab` (GitLab CLI) instead of raw `git` for authenticated operations — `gh auth login` / `glab auth login` set up their own credential helper backed by the host's credential storage rather than a plaintext file.

## How do I delete a virtualhost?

If for whatever reason you want to delete a virtualhost, for instance `to-be-deleted.localhost` you need to do the following:

* Delete the folder where the files are located

```shell
sudo rm -rf /var/www/to-be-deleted.localhost
```

* Delete the Apache configuration file

```shell
sudo rm -f /etc/httpd/sites-available/to-be-deleted.localhost.conf
```

* Delete the enabled site symlink

```shell
sudo rm -f /etc/httpd/sites-enabled/to-be-deleted.localhost.conf
```

* Restart httpd server

```shell
sudo systemctl restart httpd
```

## How do I remove the provisioned stack without deleting the whole distro?

Unlike `wsl --unregister AlmaLinux-10` (which deletes the entire distro and everything in it), you can remove just the provisioned stack and keep the distro itself:

```shell
sudo dnf remove -y httpd php* mariadb-server phpMyAdmin nodejs
sudo rm -rf /var/www/*.localhost
```

Anything Composer or npm installed under your project directories goes away along with those project folders. Your `config.yml` (with its stored MariaDB root password) isn't removed automatically — delete `development/wsl/config.yml` yourself if you no longer need it.

## How do I create command aliases?

From either your terminal or file explorer, navigate to your home directory (`/home/<your-username>/`).

Using your preferred text editor, open the file: `.bash_profile` (if it does not exist, create it first).

Move to the end of the file and enter on a new line:

```text
alias command_alias="command to execute"
```

where:

* `command_alias` is the name by which you want to call your original command
* `command to execute`: the original command to be executed on alias call

### Example

```text
alias list_files="ls -Al"
```

will create an alias called `list_files` that will run the command `ls -Al`.

Then, you can execute your custom alias in a terminal just as a regular command:

```shell
list_files
```
