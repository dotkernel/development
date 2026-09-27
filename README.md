# Development environment

A local PHP development stack on **AlmaLinux 10**, compatible with RHEL, that runs inside WSL 2 on Windows or directly on a Linux host.
A single Ansible playbook installs and configures the whole stack, and a second one provisions a virtualhost for each of your projects.
It works with any PHP framework or project.

## Why use it

- **Production parity** — AlmaLinux matches the RHEL-compatible distributions commonly used in staging and production, so what runs locally behaves the same once deployed.
- **One-command setup** — go from Windows Terminal through WSL 2 to a fully provisioned AlmaLinux 10 via Ansible, with no manual configuration.
- **Automatic virtualhosts** — every project gets its own `*.localhost` subdomain, with no edits to the Windows `hosts` file.
- **Version switching** — shell aliases switch between PHP 8.1–8.5 and Node.js 18–24 in a single command.

## What you get

Running `install.yml` installs and configures:

| Component | Details |
| --- | --- |
| Web server | Apache, routing `*.localhost` domains to your projects |
| PHP | PHP-FPM 8.5 by default; 8.1–8.5 available via the `php81` … `php85` aliases |
| Database | MariaDB 12.3, plus phpMyAdmin |
| Node.js | 22 by default; 18–24 available via the `node18` … `node24` aliases |
| Tools | Composer, Git (configured with your name and email) and the required Ansible collections |

The pinned versions live in `wsl/roles/php/tasks/main.yml`, `wsl/roles/mariadb/templates/MariaDB.repo.j2` and `wsl/roles/nodejs/tasks/main.yml`.

## Quick start

The full step-by-step guide is at <https://docs.dotkernel.org/development/v2/>; the outline is below.

1. **Check the requirements** — in Windows Terminal, confirm WSL 2 is available with `wsl -v`.
   See [System Requirements](https://docs.dotkernel.org/development/v2/setup/system-requirements/).
2. **Install AlmaLinux 10** — still in Windows Terminal, run `wsl --install -d AlmaLinux-10` and create your Unix user.
   See [Installation](https://docs.dotkernel.org/development/v2/setup/installation/).
3. **Run the playbook** — inside AlmaLinux 10, install Ansible and the system packages, clone this repository, create your `config.yml` and run the installer:

    ```shell
    sudo dnf install epel-release dnf-utils https://rpms.remirepo.net/enterprise/remi-release-$(rpm -E %almalinux).rpm -y
    sudo dnf install ansible-core -y
    ansible-galaxy collection install community.general community.mysql
    cd ~
    git clone --branch alma-linux-10 --single-branch https://github.com/dotkernel/development.git
    cd development/wsl/
    cp config.yml.dist config.yml
    ansible-playbook -i hosts install.yml --ask-become-pass
    ```

    Fill in your Git identity and MariaDB root password in `config.yml` before running the playbook.
    See [Setup Packages](https://docs.dotkernel.org/development/v2/setup/setup-packages/).

New to Windows Terminal?
Start with [Terminal](https://docs.dotkernel.org/development/v2/terminal/).
On a native Linux host, skip steps 1 and 2 and go straight to [Setup Packages](https://docs.dotkernel.org/development/v2/setup/setup-packages/).

> This stack is for **local development only**.
> Security is relaxed by default: the MariaDB root password is stored in plaintext in `config.yml`, and phpMyAdmin allows root login.
> Never expose it to a network.

## Virtualhosts

List the domains you want under the `virtualhosts` key in `wsl/config.yml`, then run:

```shell
ansible-playbook -i hosts create-virtualhost.yml --ask-become-pass
```

Each domain gets its own directory at `/var/www/<domain>/html`, with Apache serving its `public/` subdirectory as the document root.
Virtualhosts that already exist are skipped, so you can re-run the playbook whenever you add a project.
See [Virtualhosts](https://docs.dotkernel.org/development/v2/virtualhosts/overview/) for details.

## Which version should I use?

Two versions of this guide are maintained:

- **v2** targets **AlmaLinux 10** and is the current, actively maintained version — start here unless you have a specific reason not to.
- **v1** targets **AlmaLinux 9**, for environments that haven't moved to AlmaLinux 10 yet.

## Resources

- [Full documentation](https://docs.dotkernel.org/development/v2/)
- [Landing page](https://www.dotkernel.com/wsl2/)
- [FAQ](https://docs.dotkernel.org/development/v2/faq/)
- [Contact the Dotkernel team](https://www.dotkernel.com/contact/)
