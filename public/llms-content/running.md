---
title: Running AlmaLinux 10 on WSL 2
description: Start (or reconnect to) your AlmaLinux 10 distro from Windows Terminal, check whether it's currently running, and shut it down when you're done.
author: "admin"
date_published: "2026-07-27"
canonical_url: "https://docs.dotkernel.org/development/v2/running"
category: "Development"
language: "en"
---

# Running AlmaLinux 10 on WSL 2

## TL;DR

Start (or reconnect to) your AlmaLinux 10 distro from Windows Terminal with `wsl -d AlmaLinux-10` or the tab selector dropdown, check whether it's currently running with `wsl -l -v`, and shut it down with `wsl -t AlmaLinux-10` (or `wsl --shutdown` for all distros) when you're done.

## FAQ

### What if `AlmaLinux-10` doesn't appear in the tab selector dropdown?

Confirm it's actually installed by running:

```shell
wsl -l -v
```

If it's missing from the list entirely, revisit [Install AlmaLinux 10](setup/installation.md).
Restarting Windows Terminal after installation also helps it pick up the new tab profile.

### What if `wsl -d AlmaLinux-10` fails to start?

Run the below command to confirm the distro is installed and check its current state:

```shell
wsl -l -v
```

If another distro is using the same resources, stop it first with the below command, then retry:

```shell
wsl -t <distro-name>
```
