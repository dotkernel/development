---
title: AlmaLinux 10 Setup
description: Update system packages, install Ansible and required repositories, clone this repository, configure config.yml, and run the install.yml Ansible playbook to provision the full development environment.
author: "admin"
date_published: "2026-07-27"
canonical_url: "https://docs.dotkernel.org/development/v2/setup/setup-packages"
category: "Development"
language: "en"
---

# AlmaLinux 10 Setup

## TL;DR

Update system packages, install Ansible and required repositories, clone this repository, configure `config.yml`, and run the `install.yml` Ansible playbook to provision the full development environment. All commands on this page run **inside the AlmaLinux 10 shell** (Linux), not in Windows Terminal. This is step 3 of 3, after [System Requirements](system-requirements.md) and [Install AlmaLinux 10](installation.md), and it also works without WSL.

## FAQ

### What if `config.yml` doesn't exist yet?

Duplicate the provided template by running:

```shell
cp config.yml.dist config.yml
```

Then fill in the empty fields before running the playbook.

### What if the Ansible playbook fails partway through?

Review the summary output for the failed task, fix the underlying issue (for example, a misconfigured field in `config.yml`), and re-run:

```shell
ansible-playbook -i hosts install.yml --ask-become-pass
```

It is safe to re-run.

### What if `ansible-galaxy collection install` fails or times out?

This command downloads collections from Ansible Galaxy over the internet, so it fails if your network connection is down, unstable, or blocked by a proxy/firewall.
Check your connection and re-run the command.
It's safe to run again even if some collections already installed successfully.

### What if `http://localhost/` doesn't load after installation?

If you are not using WSL 2, make sure you are using your server's IP address instead of `localhost`.
Otherwise, confirm the playbook completed without errors and that your **AlmaLinux 10** distro is still running.

### How do I log into phpMyAdmin?

Use the username `root` and the password you configured under `mariadb` -> `root_password` in `config.yml`.

### Can I run this setup without WSL?

Yes, the instructions work the same way on a non-WSL AlmaLinux system.
You can then skip the WSL-specific notes.
