# Development environment

This repo provisions a full local development environment for any PHP framework or project, using an Ansible playbook, runnable directly on Linux or inside WSL 2 on Windows.

## What you get

Running the playbook installs and configures:

* Apache
* PHP-FPM (version pinned in `wsl/roles/php/tasks/main.yml`)
* MariaDB (version pinned in `wsl/roles/mariadb/templates/MariaDB.repo.j2`)
* phpMyAdmin
* Composer
* Node.js 22

## Which version should I use?

Two versions of this guide are maintained:

* **v2** targets **AlmaLinux 10** and is the current, actively maintained version - start here unless you have a specific reason not to.
* **v1** targets **AlmaLinux 9**, for environments that haven't moved to AlmaLinux 10 yet.

## Getting started

Full documentation is published at <https://docs.dotkernel.org/development/v2/>.

If you're using WSL 2 (Windows Subsystem for Linux) to run your development environment, start with [Terminal](https://docs.dotkernel.org/development/v2/terminal/) to install Windows Terminal, then continue to [System Requirements](https://docs.dotkernel.org/development/v2/setup/system-requirements/).

If you're not using WSL (for example, a native Linux host), you can jump straight to [Setup Packages](https://docs.dotkernel.org/development/v2/setup/setup-packages/).
