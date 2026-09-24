# WSL Configuration

## Summary

Where to put your project files, how to open them from Windows, how to tune WSL 2's resource usage, and how to back up or move your distro.

## Where to put your project files

Keep your projects inside the WSL filesystem - for example under `/var/www/`, which is exactly where this playbook creates your virtualhosts. Avoid working with projects stored under `/mnt/c/...` (or any other Windows drive mounted into WSL): crossing filesystems this way can significantly slow down file-heavy operations like `composer install`, `npm install`, or a framework's file-watcher.

> If you cloned a project onto `C:\` before setting up WSL, move or re-clone it into your AlmaLinux 10 home directory or `/var/www` instead.

## Opening a WSL project from Windows

To browse your **AlmaLinux 10** files from Windows File Explorer, open:

```text
\\wsl.localhost\AlmaLinux-10\var\www\
```

(older WSL versions use `\\wsl$\AlmaLinux-10\...` instead - both work).

From inside AlmaLinux 10, you can also open the current directory in Windows File Explorer directly:

```shell
explorer.exe .
```

## `.wslconfig`

WSL 2 runs inside a lightweight VM, and by default it can use most of your machine's memory and grow its virtual disk indefinitely. You can cap this with a `.wslconfig` file at `C:\Users\<your-windows-username>\.wslconfig` (created from Windows, not inside AlmaLinux 10):

```text
[wsl2]
memory=8GB
processors=4
swap=2GB
vmIdleTimeout=60000
networkingMode=mirrored
```

* `memory` / `processors`: cap how much RAM/CPU the WSL VM can use.
* `swap`: swap file size inside the VM.
* `vmIdleTimeout`: milliseconds of inactivity before the VM is torn down (frees memory when you're not using WSL).
* `networkingMode=mirrored`: mirrors your Windows network interfaces into WSL, which can fix some VPN/corporate-network connectivity issues.

Apply changes with:

```shell
wsl --shutdown
```

then reopen **AlmaLinux 10**.

> The VM's virtual disk (VHDX) only grows - it doesn't shrink automatically as you delete files. See Microsoft's guide on compacting a WSL disk if it grows larger than expected.

## Backup and portability

`wsl --unregister AlmaLinux-10` (documented in [Install AlmaLinux 10](setup/installation.md)) deletes the distro and everything in it - it's a reset, not a backup.

To back up or move your environment instead, export it to a single file:

```shell
wsl --export AlmaLinux-10 alma-linux-10-backup.tar
```

And restore it (to the same machine or a different one) with:

```shell
wsl --import AlmaLinux-10 C:\WSL\AlmaLinux-10 alma-linux-10-backup.tar
```

## Next step

See [Editor Integration](editor-integration.md) to connect VS Code or PhpStorm, or jump to the [FAQ](faq.md) for common troubleshooting.
