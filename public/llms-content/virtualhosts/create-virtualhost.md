---
title: AlmaLinux 10 Create virtualhosts
description: Add domains to config.yml and run the create-virtualhost.yml Ansible playbook to provision new virtualhosts on AlmaLinux 10.
author: "admin"
date_published: "2026-07-27"
canonical_url: "https://docs.dotkernel.org/development/v2/virtualhosts/create-virtualhost"
category: "Development"
language: "en"
---

# AlmaLinux 10 Create virtualhosts

## TL;DR

Add your desired domains to `config.yml` and run the `create-virtualhost.yml` Ansible playbook to provision each one. This assumes [Setup Packages](../setup/setup-packages.md) is already complete, since Apache must already be installed before virtualhosts can be created. Using the `*.localhost` pattern means no changes to the Windows `hosts` file are needed.

## FAQ

### What if I don't have a `config.yml` file yet?

Duplicate the provided template by running the below command inside `development/wsl`:

```shell
cp config.yml.dist config.yml
```

Then add your virtualhosts under the `virtualhosts` key.

### What happens if I list a virtualhost that already exists?

It will simply be skipped.
Its existing files and configuration are left untouched, so there's no need to comment out or remove already-created entries from `config.yml`.

### Where should I put my project's files?

Under the `html` directory of your virtualhost, for example `/var/www/example.localhost/html`.
The document root is set to the `public` subdirectory of that location, for example `/var/www/example.localhost/html/public`.

### How do I make the DocumentRoot point directly to `html` instead of `html/public`?

Edit the virtualhost's Apache configuration file at `/etc/httpd/sites-available/example.localhost` and change the `DocumentRoot` accordingly.

### How do I access my virtualhost if AlmaLinux 10 isn't running?

You need to start **AlmaLinux 10** first.
Your virtualhosts are only reachable while the distro is running.

### What if I hit permission errors when writing to my project's files?

See the [permission issues guide](https://docs.dotkernel.org/development/v2/faq/#how-do-i-fix-common-permission-issues) in the FAQ.

### How do I delete a virtualhost I no longer need?

See [How do I delete a virtualhost?](https://docs.dotkernel.org/development/v2/faq/#how-do-i-delete-a-virtualhost) in the FAQ.
